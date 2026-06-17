# Registration through a developer portal

*Worked example for [the record location and data access write-up](../authorizing-access.md). Most of this flow is a human in a browser, so the artifacts are the two machine-verifiable pieces: the statement link the developer pastes, and a key-possession proof the portal can ask for.*

**What the developer pastes into the portal form:**

```
https://library.medicare.gov/app-library/apps/bp-buddy/software-statement.jwt
```

The portal fetches it, verifies the CMS signature against CMS's published JWKS, checks `library_status: active`, and pre-fills app name, URIs, and contacts from the payload (see [software-statement](software-statement.md)).

---

One way Alpha can verify key possession during signup is a short-lived JWT the developer's tooling produces, verifiable against the app's CMS-verified `jwks_uri`:

**key-possession proof** (compact JWS, really signed):

```
eyJhbGciOiJSUzM4NCIsImtpZCI6ImxwRmdBZlZXdzZHWm5tWUtsSzczZDFXUjlSb0hvcWxacWFRY0NiaHRiam8iLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2xpYnJhcnkubWVkaWNhcmUuZ292L2FwcC1saWJyYXJ5L2FwcHMvYnAtYnVkZHkiLCJzdWIiOiJodHRwczovL2xpYnJhcnkubWVkaWNhcmUuZ292L2FwcC1saWJyYXJ5L2FwcHMvYnAtYnVkZHkiLCJhdWQiOiJodHRwczovL2RldmVsb3BlcnMuYWxwaGEtaGVhbHRoLmV4YW1wbGUiLCJpYXQiOjE3ODEyNzE5NTMsImV4cCI6MTc4MTI3MjI1MywianRpIjoiZmVlOThkOGMtOGNiZC00Yjg2LWFmYjgtN2M4NGQ3OGUzMDUzIn0.zGkys8LKeGAganMv_HXwshATWiJSufIrtfziPZ3MGYl3kDWzB_q-U8TV7YmmSzeD7MwCXqo_lHlGa562I9nGRDEYe6hWDArmKuy_v3hrccxDNW5uK9MHqVDyhJYTqk1LC52jYEfBNQiXMPbrB3nVWGsZwX8wOVVz5ZbR_k7BrhmtjSUrN4nE0KqWsEwLvgHHU8Hx9j3kN0yfuCMb7CmHWs8PjtoIoKTVuGYaN5wwRVWUvYonrg8cNeWdQfsnORQqh1R370JCOSngCjzaG0xUy9j1AdQhji_VPBGN1NzxqRzqyu-MjB7oJWWSy_MMBajKo1AQdSjEHpoondXKMqPxiA
```

Decoded header:

```json
{
  "alg": "RS384",
  "kid": "lpFgAfVWw6GZnmYKlK73d1WR9RoHoqlZqaQcCbhtbjo",
  "typ": "JWT"
}
```

Decoded payload:

```json
{
  "iss": "https://library.medicare.gov/app-library/apps/bp-buddy",
  "sub": "https://library.medicare.gov/app-library/apps/bp-buddy",
  "aud": "https://developers.alpha-health.example",
  "iat": 1781271953,
  "exp": 1781272253,
  "jti": "fee98d8c-8cbd-4b86-afb8-7c84d78e3053"
}
```

---

**Portal provisions the registration — the developer sees**

```http
HTTP/1.1 201 Created
Content-Type: application/json
```

```json
{
  "client_id": "alpha-net-bp-buddy-7c31",
  "grant_types": [
    "client_credentials"
  ],
  "token_endpoint": "https://auth.alpha-health.example/v1/token",
  "note": "Valid at every Alpha data holder; no further registrations on this network."
}
```

*Generated 2026-06-12T13:45:53.000Z by `tools/artifact-generator` in this repository. Keys are throwaway examples; every signature verifies against the keys in [keys-and-trust-anchors](keys-and-trust-anchors.md).*