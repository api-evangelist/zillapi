---
name: verify-zillow-webhook
description: Verify the HMAC-SHA256 signature on an inbound Zillapi webhook so you can trust the payload before acting on it.
version: 1.0.0
---

# Verify Zillapi Webhook

Use this skill when your application receives an inbound webhook from Zillapi (job completion, listing alert, etc.) and you need to verify it actually came from Zillapi before processing.

## When to use

- You have set up an outbound webhook subscription via `POST /v1/webhooks`
- You receive an HTTP request at your endpoint with header `x-zillow-signature`
- You need to verify the signature before trusting the body

## How signatures work

Zillapi signs every outbound webhook with HMAC-SHA256. The signing secret is returned exactly once at subscription-creation time, so you must store it on your end.

The header format is:

```
x-zillow-signature: t=<unix-timestamp>,v1=<hex-digest>
```

The digest is computed as:

```
HMAC-SHA256(secret, "<unix-timestamp>.<request-body>")
```

The timestamp is included to prevent replay attacks. Reject signatures older than 5 minutes.

## Procedure

### Node.js

```js
import crypto from "node:crypto";

function verifyZillapiWebhook(rawBody, signatureHeader, secret) {
  const parts = Object.fromEntries(
    signatureHeader.split(",").map((p) => p.split("="))
  );
  const t = parts.t;
  const v1 = parts.v1;
  if (!t || !v1) return false;

  const ageSeconds = Math.floor(Date.now() / 1000) - parseInt(t, 10);
  if (ageSeconds > 300) return false;

  const expected = crypto
    .createHmac("sha256", secret)
    .update(`${t}.${rawBody}`)
    .digest("hex");

  return crypto.timingSafeEqual(
    Buffer.from(v1, "hex"),
    Buffer.from(expected, "hex")
  );
}
```

### Python

```python
import hmac, hashlib, time

def verify_zillapi_webhook(raw_body: bytes, signature_header: str, secret: str) -> bool:
    parts = dict(p.split("=", 1) for p in signature_header.split(","))
    t, v1 = parts.get("t"), parts.get("v1")
    if not t or not v1: return False
    if int(time.time()) - int(t) > 300: return False
    expected = hmac.new(secret.encode(), f"{t}.{raw_body.decode()}".encode(), hashlib.sha256).hexdigest()
    return hmac.compare_digest(v1, expected)
```

### Go

```go
import (
    "crypto/hmac"
    "crypto/sha256"
    "encoding/hex"
    "strconv"
    "strings"
    "time"
)

func VerifyZillapiWebhook(body []byte, sigHeader, secret string) bool {
    parts := map[string]string{}
    for _, kv := range strings.Split(sigHeader, ",") {
        if k, v, ok := strings.Cut(kv, "="); ok { parts[k] = v }
    }
    t, err := strconv.ParseInt(parts["t"], 10, 64)
    if err != nil || time.Now().Unix()-t > 300 { return false }
    mac := hmac.New(sha256.New, []byte(secret))
    mac.Write([]byte(parts["t"] + "." + string(body)))
    expected := hex.EncodeToString(mac.Sum(nil))
    return hmac.Equal([]byte(expected), []byte(parts["v1"]))
}
```

## Critical rules

1. **Use the raw request body**, not a re-serialized JSON. Even one byte of difference breaks the signature.
2. **Use a constant-time comparison** (`crypto.timingSafeEqual`, `hmac.compare_digest`). String equality leaks timing information.
3. **Reject old signatures**. The spec says 5 minutes; tighten to 1 minute if your endpoint is high-traffic.
4. **Never log the signing secret**. It is recoverable only by rotation.

## Event types

Zillapi delivers these events:

- `job.succeeded`: async job completed; results available via `/v1/jobs/{id}/results`
- `job.failed`: async job failed; check `error` field
- `job.timed_out`: job exceeded the time budget
- `job.aborted`: job manually aborted

## Idempotency

Webhooks may be retried up to 5 times with exponential backoff if your endpoint returns non-2xx. Always handle the same event-id idempotently: store delivered event ids and short-circuit duplicates.

## Reference

- Webhook reference: https://zillapi.com/api/webhooks/
- Recipe with full example: https://zillapi.com/recipes/verify-webhook/
