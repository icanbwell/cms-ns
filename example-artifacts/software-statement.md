# The CMS-signed software statement

*Worked example for [the record location and data access write-up](../authorizing-access.md). CMS re-signs this statement on a short cycle for as long as BP Buddy is active in the Medicare App Library. It is the only credential the app carries into every network.*

**Request — anyone may fetch the current statement**

```http
GET https://library.medicare.gov/app-library/apps/bp-buddy/software-statement.jwt HTTP/1.1
Accept: application/jwt
```

---

**Response** `200 OK`, `Content-Type: application/jwt`

**software_statement** (compact JWS, really signed):

```
eyJhbGciOiJFUzM4NCIsImtpZCI6Ino3MUE1Zi1KLWhleDA0cWk3LVhiV0ZDZjE3S1YxVDlpdnJ1YnFfY2hVb00iLCJ0eXAiOiJKV1QifQ.eyJzb2Z0d2FyZV9pZCI6Imh0dHBzOi8vbGlicmFyeS5tZWRpY2FyZS5nb3YvYXBwLWxpYnJhcnkvYXBwcy9icC1idWRkeSIsImNsaWVudF9uYW1lIjoiQlAgQnVkZHkiLCJjbGllbnRfdXJpIjoiaHR0cHM6Ly9icGJ1ZGR5LmV4YW1wbGUiLCJwb2xpY3lfdXJpIjoiaHR0cHM6Ly9icGJ1ZGR5LmV4YW1wbGUvcHJpdmFjeSIsImNvbnRhY3RzIjpbInN1cHBvcnRAYnBidWRkeS5leGFtcGxlIl0sImdyYW50X3R5cGVzIjpbImNsaWVudF9jcmVkZW50aWFscyJdLCJ0b2tlbl9lbmRwb2ludF9hdXRoX21ldGhvZCI6InByaXZhdGVfa2V5X2p3dCIsImp3a3NfdXJpIjoiaHR0cHM6Ly9icGJ1ZGR5LmV4YW1wbGUvLndlbGwta25vd24vandrcy5qc29uIiwiZXh0ZW5zaW9ucyI6eyJjbXNfYXBwIjp7InZlcnNpb24iOiIxIiwibGlicmFyeV9zdGF0dXMiOiJhY3RpdmUiLCJhcHBfY2xhc3MiOiJwYXRpZW50LWFjY2Vzcy1hcHAifX0sImlzcyI6Imh0dHBzOi8vbGlicmFyeS5tZWRpY2FyZS5nb3YiLCJzdWIiOiJodHRwczovL2xpYnJhcnkubWVkaWNhcmUuZ292L2FwcC1saWJyYXJ5L2FwcHMvYnAtYnVkZHkiLCJhdWQiOiJodHRwczovL2ZyYW1ld29yay5jbXMuZ292L2FsaWduZWQtbmV0d29ya3MiLCJpYXQiOjE3ODEyNzE5NTMsImV4cCI6MTc4MTM1ODM1MywianRpIjoiOWEzZWY5NzQtOWY2Zi00ZDI3LWFjZDktMGI3MTcwMjU0ZTM0In0.1SzJWgGjpOSR8C3Qo4qQxNttm3Dj4zCT9l5pxVoZdclHQmN4jqyDwcytlyhB7RnH_HlK2gzd2212qLnXY6xXbjaT6KLoJF6-lYBn_Edz1JDON1u5sDQ1N_ppWU7OfEH_
```

Decoded header:

```json
{
  "alg": "ES384",
  "kid": "z71A5f-J-hex04qi7-XbWFCf17KV1T9ivrubq_chUoM",
  "typ": "JWT"
}
```

Decoded payload:

```json
{
  "software_id": "https://library.medicare.gov/app-library/apps/bp-buddy",
  "client_name": "BP Buddy",
  "client_uri": "https://bpbuddy.example",
  "policy_uri": "https://bpbuddy.example/privacy",
  "contacts": [
    "support@bpbuddy.example"
  ],
  "grant_types": [
    "client_credentials"
  ],
  "token_endpoint_auth_method": "private_key_jwt",
  "jwks_uri": "https://bpbuddy.example/.well-known/jwks.json",
  "extensions": {
    "cms_app": {
      "version": "1",
      "library_status": "active",
      "app_class": "patient-access-app"
    }
  },
  "iss": "https://library.medicare.gov",
  "sub": "https://library.medicare.gov/app-library/apps/bp-buddy",
  "aud": "https://framework.cms.gov/aligned-networks",
  "iat": 1781271953,
  "exp": 1781358353,
  "jti": "9a3ef974-9f6f-4d27-acd9-0b7170254e34"
}
```

---

**The app's published JWKS** at `https://bpbuddy.example/.well-known/jwks.json` (CMS verified control of this URL at admission and monitors it afterward):

```json
{
  "keys": [
    {
      "e": "AQAB",
      "kty": "RSA",
      "n": "2k6y2J17IyNYzWwRfcgY4hjMTe4TUihx3D8owWJFBoQYafy0Sa8FlX6h6aFrSWB5Os6e8ulnTVkOM6MvJWyV2L81w4z4H1_LRzC8cWo1PIs3At4Kw0OeOmTzXiw9Tx4B6oESXUrb5oA9Iu_a1r1PYbKfSqIss0w2emDuMAcMJARw7C3BQ6_qDGlFWmGUL2pVaKZfD-j6jMAfnE1nZBYN9O64iNdXm2SwENvpoIT7xuxaLjeui8hSunBEGeKDIJO3sQKRBneX7RBlxiHoWKjCX6Ac_BtvQ9Sjz0rPLby2QOjxPb7trOJFVHmKSHs_4Ki23Wae_Bl765uY4nbQX2X8DQ",
      "alg": "RS384",
      "use": "sig",
      "kid": "lpFgAfVWw6GZnmYKlK73d1WR9RoHoqlZqaQcCbhtbjo"
    }
  ]
}
```

*Generated 2026-06-12T13:45:53.000Z by `tools/artifact-generator` in this repository. Keys are throwaway examples; every signature verifies against the keys in [keys-and-trust-anchors](keys-and-trust-anchors.md).*