# The app signs Maria in at the CSP

*Worked example for [the record location and data access write-up](../authorizing-access.md). BP Buddy is the CSP's relying party. The id_token it receives carries the app's canonical Library identifier as its audience, which is what lets any later verifier resolve the token to this app.*

**Authorize request (browser redirect to the CSP)**

```http
GET https://api.id.me/oidc/authorize?response_type=code HTTP/1.1
  &client_id=https%3A%2F%2Flibrary.medicare.gov%2Fapp-library%2Fapps%2Fbp-buddy
  &redirect_uri=https://bpbuddy.example/csp/callback
  &scope=openid+profile
  &state=af0X9 &nonce=n-7qL2
```

---

**Token response after the code exchange**

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "access_token": "0cCfy2Pr664Z51vQ1aPFjxnEumgCSenh",
  "token_type": "Bearer",
  "expires_in": 300,
  "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6ImVOdTFWdEl3R3hQVkZPOF... (decoded below)"
}
```

---

**id_token: aud is the app's Library software_id** (compact JWS, really signed):

```
eyJhbGciOiJSUzI1NiIsImtpZCI6ImVOdTFWdEl3R3hQVkZPOFJXdkEzVDVSam1MMnl0TzdaQnNHaU5KQkRQTE0iLCJ0eXAiOiJKV1QifQ.eyJpZGVudGl0eV9hc3N1cmFuY2VfbGV2ZWwiOjIsImF1dGhfdGltZSI6MTc4MTI3MTg5MywiZ2l2ZW5fbmFtZSI6Ik1hcmlhIiwiZmFtaWx5X25hbWUiOiJMb3BleiIsImJpcnRoZGF0ZSI6IjE5NjItMDMtMTUiLCJhZGRyZXNzIjp7InN0cmVldF9hZGRyZXNzIjoiNDE4IEFsZGVyIENvdXJ0IiwibG9jYWxpdHkiOiJSaXZlcnNpZGUiLCJyZWdpb24iOiJDQSIsInBvc3RhbF9jb2RlIjoiOTI1MDEiLCJjb3VudHJ5IjoiVVMifSwic3NuX2l0aW5fc2hvcnQiOiI0MzIxIiwiaXNzIjoiaHR0cHM6Ly9hcGkuaWQubWUvb2lkYyIsInN1YiI6IjkyMzdlNjRhLWU2MmEtNDVkMS04ZGIxLTVjMDMxMWE1MzIwYyIsImF1ZCI6Imh0dHBzOi8vbGlicmFyeS5tZWRpY2FyZS5nb3YvYXBwLWxpYnJhcnkvYXBwcy9icC1idWRkeSIsImlhdCI6MTc4MTI3MTg5MywiZXhwIjoxNzgxMjcyMTkzLCJqdGkiOiI1YWE1YmFiNC02ZDgzLTQxNTYtODYxZS0xZDM3NWI5ZWQ4NWMifQ.d2HCKMtUh8RKhPbJd3Ovq-ipssxjr8xn09Yz4ML8OK2qCEWRyCHWg4jCBFroLR99qMsyyNhexTRUE-28xtE-0-Psg3A9NmKMUwyYqPWkulVE4hTALR_YRNb3Qxfc2I5F1PScIv97buDchPGdP9HTuNfi58pOpFeczbv14lJafQq4bVBQ84yh4FjuI_LCfSUXC5KyuHKo9vzHeyNYWr1yGXDCih8BgG82mcQoi5Apz77jIRZwWo61cKRkuHJnNMlh57R20qXcyfYEpAkfKQnYwKirz8PFhazOp2Xd0LdRThCP5_fdWPZiAWGI34VIZHDOWYaAypJZ9zbX1u7We2g4wg
```

Decoded header:

```json
{
  "alg": "RS256",
  "kid": "eNu1VtIwGxPVFO8RWvA3T5RjmL2ytO7ZBsGiNJBDPLM",
  "typ": "JWT"
}
```

Decoded payload:

```json
{
  "identity_assurance_level": 2,
  "auth_time": 1781271893,
  "given_name": "Maria",
  "family_name": "Lopez",
  "birthdate": "1962-03-15",
  "address": {
    "street_address": "418 Alder Court",
    "locality": "Riverside",
    "region": "CA",
    "postal_code": "92501",
    "country": "US"
  },
  "ssn_itin_short": "4321",
  "iss": "https://api.id.me/oidc",
  "sub": "9237e64a-e62a-45d1-8db1-5c0311a5320c",
  "aud": "https://library.medicare.gov/app-library/apps/bp-buddy",
  "iat": 1781271893,
  "exp": 1781272193,
  "jti": "5aa5bab4-6d83-4156-861e-1d375b9ed85c"
}
```

*Generated 2026-06-12T13:45:53.000Z by `tools/artifact-generator` in this repository. Keys are throwaway examples; every signature verifies against the keys in [keys-and-trust-anchors](keys-and-trust-anchors.md).*