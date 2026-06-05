# Gmail Attachment Download — Bug Fix Notes

**Date:** 2026-05-30
**Context:** Node.js script using `https.request` to download Gmail attachments via the Gmail REST API.

---

## Bug 1 — Incorrect `https.request` Signature

### Root Cause

`https.request` accepts either:
- `https.request(options, callback)`, or
- `https.request(url, options, callback)`

The original code passed the URL, then an options object, then the callback — but the second argument was being treated as the callback, so the `Authorization` header was never sent.

```js
// ❌ Broken — options object is silently dropped
const req = https.request(opts, { method: "GET", headers: { Authorization: `Bearer ${token}` } }, (res) => { ... });
```

### Fix

Construct a plain options object from the parsed URL and pass `(options, callback)`:

```js
// ✅ Fixed
function httpGet(url, token) {
  return new Promise((resolve, reject) => {
    const urlObj = new URL(url);
    const opts = {
      hostname: urlObj.hostname,
      path: urlObj.pathname + urlObj.search,
      method: "GET",
      headers: { Authorization: `Bearer ${token}` },
    };
    const req = https.request(opts, (res) => {
      const chunks = [];
      res.on("data", (d) => chunks.push(d));
      res.on("end", () => {
        const buf = Buffer.concat(chunks);
        resolve({ status: res.statusCode, headers: res.headers, body: buf });
      });
    });
    req.on("error", reject);
    req.end();
  });
}
```

---

## Bug 2 — Wrong Base64 Encoding Variant

### Root Cause

The Gmail API returns attachment data encoded as **base64url** (RFC 4648 §5), which uses `-` and `_` instead of `+` and `/`. Node's `Buffer.from(data, "base64")` uses standard base64 and silently misinterprets those characters, producing corrupted or truncated files.

```js
// ❌ Broken — produces corrupted output on any attachment with + or / chars
fs.writeFileSync(dest, Buffer.from(data, "base64"));
```

### Fix

```js
// ✅ Fixed
fs.writeFileSync(dest, Buffer.from(data, "base64url"));
```

---

## Additional Recommendation — Token Refresh

The `access_token` in `/root/.gmail_token.json` has a short TTL (typically 1 hour). If the script runs later or is retried, the token may be expired, causing a `401 Unauthorized` on the download call even if the `gmail` CLI wrapper (which uses its own auth flow) works fine.

**Recommendation:** Before calling `downloadAttachment`, check token expiry and refresh if needed:

```js
const tokenJson = JSON.parse(fs.readFileSync(TOKEN_PATH, "utf8"));
const expiresAt = tokenJson.expiry_date || tokenJson.expires_at || 0;
if (Date.now() >= expiresAt - 60_000) {
  // Refresh via OAuth2 client before proceeding
  // e.g., use google-auth-library or a manual POST to https://oauth2.googleapis.com/token
}
```

---

## Summary Table

| # | Issue | Location | Fix |
|---|-------|----------|-----|
| 1 | `https.request` options silently dropped | `httpGet()` | Pass `(options, callback)` — not `(url, options, callback)` |
| 2 | `base64` used instead of `base64url` | `downloadAttachment()` | `Buffer.from(data, "base64url")` |
| 3 | `access_token` may be expired | `downloadAttachment()` | Add expiry check + token refresh before download |
