# Opening the authorization step

*Worked example for [the record location and data access write-up](../authorizing-access.md). The app opens a standard SMART App Launch code flow at the shared authorization service, passing Maria's id_token as a hint. The service then re-authenticates her silently at the CSP and receives a fresh id_token audienced to itself.*

**The app's authorize request at the service (browser redirect)**

```http
GET https://issuer.beta-exchange.example/authorize?response_type=code HTTP/1.1
  &client_id=sas-bp-buddy-3f81
  &redirect_uri=https://bpbuddy.example/callback
  &scope=permission_ticket+patient%2FObservation.rs+offline_access
  &code_challenge=E9Mt... &code_challenge_method=S256 &state=x7Hq
  &id_token_hint=eyJhbGciOiJSUzI1NiIsImtpZCI6ImVOdTFWdEl3...
```

---

**The service's silent re-authentication at the CSP (no screen if Maria's CSP session is live)**

```http
GET https://api.id.me/oidc/authorize?response_type=code&prompt=none HTTP/1.1
  &client_id=https%3A%2F%2Fissuer.beta-exchange.example
  &redirect_uri=https://issuer.beta-exchange.example/csp/callback
  &scope=openid
  &id_token_hint=eyJhbGciOiJSUzI1NiIsImtpZCI6ImVOdTFWdEl3...
```

---

**Fresh id_token from the silent re-auth: same person, new auth event, aud is now the service** (compact JWS, really signed):

```
eyJhbGciOiJSUzI1NiIsImtpZCI6ImVOdTFWdEl3R3hQVkZPOFJXdkEzVDVSam1MMnl0TzdaQnNHaU5KQkRQTE0iLCJ0eXAiOiJKV1QifQ.eyJpZGVudGl0eV9hc3N1cmFuY2VfbGV2ZWwiOjIsImF1dGhfdGltZSI6MTc4MTI3MTkyMywiZ2l2ZW5fbmFtZSI6Ik1hcmlhIiwiZmFtaWx5X25hbWUiOiJMb3BleiIsImJpcnRoZGF0ZSI6IjE5NjItMDMtMTUiLCJpc3MiOiJodHRwczovL2FwaS5pZC5tZS9vaWRjIiwic3ViIjoiYzlhZTc5ZmUtZmVmMS00NDllLWI2YzktYjdkZjBlZGJiNGUwIiwiYXVkIjoiaHR0cHM6Ly9pc3N1ZXIuYmV0YS1leGNoYW5nZS5leGFtcGxlIiwiaWF0IjoxNzgxMjcxOTIzLCJleHAiOjE3ODEyNzIyMjMsImp0aSI6ImIyZTUwMjEwLTJlYjUtNDY3Mi1hYTA1LTFjMjg0YjkzM2M5YyJ9.JahQZRx6fssLqc60DkDyWFtyA4AK2l0vlV7AhlNLQABC1AVxSiupw3vXpAiLnZfnxXDte41ebfk2yvvNTBEdkGLOkttxOcWuLDV71gFiy9ihv4o3MwBzx6UmyBWLN7_o63MJgR-uLcYeuxg2955KnbXfa4aPEeujbsm_84GTkwozKT-d5naSX_SuWqv8oxA80Z037efmJ3ufxNVaqu2Zdh29fsUzo6fFGx5UzZJQYncldrVGoEEyc3wxK8wyt_2TCIFAnZN9CKfLx-AP3JZhJAsWTz2QchUVKIlp3kJY9RelrYeumNbiMEnej2OfNC-T1y4BcqUfUwsIPS9iRiDcJg
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
  "auth_time": 1781271923,
  "given_name": "Maria",
  "family_name": "Lopez",
  "birthdate": "1962-03-15",
  "iss": "https://api.id.me/oidc",
  "sub": "c9ae79fe-fef1-449e-b6c9-b7df0edbb4e0",
  "aud": "https://issuer.beta-exchange.example",
  "iat": 1781271923,
  "exp": 1781272223,
  "jti": "b2e50210-2eb5-4672-aa05-1c284b933c9c"
}
```

---

The lighter option skips the silent re-auth: the service accepts the app-passed id_token itself as the sign-in. That token is verifiable and audience-bound to the app, and it proves the app holds a recent assertion about Maria, not that Maria is present in this browser.

*Generated 2026-06-12T13:45:53.000Z by `tools/artifact-generator` in this repository. Keys are throwaway examples; every signature verifies against the keys in [keys-and-trust-anchors](keys-and-trust-anchors.md).*