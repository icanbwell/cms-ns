# client_credentials + $rls: the app-asserted grant at a network

*Worked example for [the record location and data access write-up](../authorizing-access.md). Maria authenticated at her IAL2 CSP moments ago; her id_token travels inside the cms_smart extension of the client_assertion, following the Blue Button CMS Aligned Networks pattern. The access token comes back bound to her, so $rls can only locate her records.*

**CSP-issued IAL2 id_token (ID.me-style claims)** (compact JWS, really signed):

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

The `aud` is the app's canonical Library identifier: the app configures its CSP registration so id_tokens carry its `software_id`. Any network or data holder can then verify the relationship between the id_token's audience and the presenting application (can-spec §9) by matching `aud` against the `software_id` it bound at registration — the identifiers are literally identical, no directory needed.

---

**Token request**

```http
POST https://rls.beta-exchange.example/oauth/token HTTP/1.1
Host: rls.beta-exchange.example
Content-Type: application/x-www-form-urlencoded
```

```json
{
  "grant_type": "client_credentials",
  "scope": "patient/Patient.rs launch/patient",
  "client_assertion_type": "urn:ietf:params:oauth:client-assertion-type:jwt-bearer",
  "client_assertion": "eyJhbGciOiJSUzM4NCIsImtpZCI6ImxwRmdBZlZXdzZHWm5tWUtsSzczZDFX... (decoded below)"
}
```

---

**client_assertion — note extensions.cms_smart carrying the full id_token** (compact JWS, really signed):

```
eyJhbGciOiJSUzM4NCIsImtpZCI6ImxwRmdBZlZXdzZHWm5tWUtsSzczZDFXUjlSb0hvcWxacWFRY0NiaHRiam8iLCJ0eXAiOiJKV1QifQ.eyJleHRlbnNpb25zIjp7ImNtc19zbWFydCI6eyJ2ZXJzaW9uIjoiMSIsInB1cnBvc2Vfb2ZfdXNlIjoiUEFUUlFUIiwiaWRfdG9rZW4iOiJleUpoYkdjaU9pSlNVekkxTmlJc0ltdHBaQ0k2SW1WT2RURldkRWwzUjNoUVZrWlBPRkpYZGtFelZEVlNhbTFNTW5sMFR6ZGFRbk5IYVU1S1FrUlFURTBpTENKMGVYQWlPaUpLVjFRaWZRLmV5SnBaR1Z1ZEdsMGVWOWhjM04xY21GdVkyVmZiR1YyWld3aU9qSXNJbUYxZEdoZmRHbHRaU0k2TVRjNE1USTNNVGc1TXl3aVoybDJaVzVmYm1GdFpTSTZJazFoY21saElpd2labUZ0YVd4NVgyNWhiV1VpT2lKTWIzQmxlaUlzSW1KcGNuUm9aR0YwWlNJNklqRTVOakl0TURNdE1UVWlMQ0poWkdSeVpYTnpJanA3SW5OMGNtVmxkRjloWkdSeVpYTnpJam9pTkRFNElFRnNaR1Z5SUVOdmRYSjBJaXdpYkc5allXeHBkSGtpT2lKU2FYWmxjbk5wWkdVaUxDSnlaV2RwYjI0aU9pSkRRU0lzSW5CdmMzUmhiRjlqYjJSbElqb2lPVEkxTURFaUxDSmpiM1Z1ZEhKNUlqb2lWVk1pZlN3aWMzTnVYMmwwYVc1ZmMyaHZjblFpT2lJME16SXhJaXdpYVhOeklqb2lhSFIwY0hNNkx5OWhjR2t1YVdRdWJXVXZiMmxrWXlJc0luTjFZaUk2SWpreU16ZGxOalJoTFdVMk1tRXRORFZrTVMwNFpHSXhMVFZqTURNeE1XRTFNekl3WXlJc0ltRjFaQ0k2SW1oMGRIQnpPaTh2YkdsaWNtRnllUzV0WldScFkyRnlaUzVuYjNZdllYQndMV3hwWW5KaGNua3ZZWEJ3Y3k5aWNDMWlkV1JrZVNJc0ltbGhkQ0k2TVRjNE1USTNNVGc1TXl3aVpYaHdJam94TnpneE1qY3lNVGt6TENKcWRHa2lPaUkxWVdFMVltRmlOQzAyWkRnekxUUXhOVFl0T0RZeFpTMHhaRE0zTldJNVpXUTROV01pZlEuZDJIQ0tNdFVoOFJLaFBiSmQzT3ZxLWlwc3N4anI4eG4wOVl6NE1MOE9LMnFDRVdSeUNIV2c0akNCRnJvTFI5OXFNc3l5TmhleFRSVUUtMjh4dEUtMC1Qc2czQTlObUtNVXd5WXFQV2t1bFZFNGhUQUxSX1lSTmIzUXhmYzJJNUYxUFNjSXY5N2J1RGNoUEdkUDlIVHVOZmk1OHBPcEZlY3pidjE0bEphZlFxNGJWQlE4NHloNEZqdUlfTENmU1VYQzVLeXVIS285dnpIZXlOWVdyMXlHWERDaWg4QmdHODJtY1FvaTVBcHo3N2pJUlp3V282MWNLUmt1SEpuTk1saDU3UjIwcVhjeWZZRXBBa2ZLUW5Zd0tpcno4UEZoYXpPcDJYZDBMZFJUaENQNV9mZFdQWmlBV0dJMzRWSVpIRE9XWWFBeXBKWjl6YlgxdTdXZTJnNHdnIn19LCJpc3MiOiJiZXRhLXJscy1icC1idWRkeS01ZDIwIiwic3ViIjoiYmV0YS1ybHMtYnAtYnVkZHktNWQyMCIsImF1ZCI6Imh0dHBzOi8vcmxzLmJldGEtZXhjaGFuZ2UuZXhhbXBsZS9vYXV0aC90b2tlbiIsImV4cCI6MTc4MTI3MjI1MywianRpIjoiMzk0ZWU1MTYtOWMxYS00MzhhLTkxMGQtMjY0MmMwMzg4MDQ5In0.DLTulspvC8JY22ecxdhOXwEcs2hevDFgIu1MiLV8g1hNUGdOWMI9jbecC-EEoQ83jTVzGE-i9M-aCCrJNp87AWFvGduhT1Pt40lAHDeCmMLG3glIIQThzMCKlfBjOs_Jyj65BlDSC670lMK7f_B7CTFOZ0l-aa6AR4oMf0rZDwrPs4LN1eQODJfyvoSUP04T58XDC16AiAYouz5wNWkb-ZRbRxFokW-UoUrU5VQAisNPRHYDZhCll8X2qRd_O0DoDBOy4ZYrKpgtXLSCGw5aJsqJilejzehsJQ7a7HikG-g1qehxiJd3Z1SFld3YhufaB_JUhdx-y3KwVV85EuweYg
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
  "extensions": {
    "cms_smart": {
      "version": "1",
      "purpose_of_use": "PATRQT",
      "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6ImVOdTFWdEl3R3hQVkZPOFJXdkEzVDVSam1MMnl0TzdaQnNHaU5KQkRQTE0iLCJ0eXAiOiJKV1QifQ.eyJpZGVudGl0eV9hc3N1cmFuY2VfbGV2ZWwiOjIsImF1dGhfdGltZSI6MTc4MTI3MTg5MywiZ2l2ZW5fbmFtZSI6Ik1hcmlhIiwiZmFtaWx5X25hbWUiOiJMb3BleiIsImJpcnRoZGF0ZSI6IjE5NjItMDMtMTUiLCJhZGRyZXNzIjp7InN0cmVldF9hZGRyZXNzIjoiNDE4IEFsZGVyIENvdXJ0IiwibG9jYWxpdHkiOiJSaXZlcnNpZGUiLCJyZWdpb24iOiJDQSIsInBvc3RhbF9jb2RlIjoiOTI1MDEiLCJjb3VudHJ5IjoiVVMifSwic3NuX2l0aW5fc2hvcnQiOiI0MzIxIiwiaXNzIjoiaHR0cHM6Ly9hcGkuaWQubWUvb2lkYyIsInN1YiI6IjkyMzdlNjRhLWU2MmEtNDVkMS04ZGIxLTVjMDMxMWE1MzIwYyIsImF1ZCI6Imh0dHBzOi8vbGlicmFyeS5tZWRpY2FyZS5nb3YvYXBwLWxpYnJhcnkvYXBwcy9icC1idWRkeSIsImlhdCI6MTc4MTI3MTg5MywiZXhwIjoxNzgxMjcyMTkzLCJqdGkiOiI1YWE1YmFiNC02ZDgzLTQxNTYtODYxZS0xZDM3NWI5ZWQ4NWMifQ.d2HCKMtUh8RKhPbJd3Ovq-ipssxjr8xn09Yz4ML8OK2qCEWRyCHWg4jCBFroLR99qMsyyNhexTRUE-28xtE-0-Psg3A9NmKMUwyYqPWkulVE4hTALR_YRNb3Qxfc2I5F1PScIv97buDchPGdP9HTuNfi58pOpFeczbv14lJafQq4bVBQ84yh4FjuI_LCfSUXC5KyuHKo9vzHeyNYWr1yGXDCih8BgG82mcQoi5Apz77jIRZwWo61cKRkuHJnNMlh57R20qXcyfYEpAkfKQnYwKirz8PFhazOp2Xd0LdRThCP5_fdWPZiAWGI34VIZHDOWYaAypJZ9zbX1u7We2g4wg"
    }
  },
  "iss": "beta-rls-bp-buddy-5d20",
  "sub": "beta-rls-bp-buddy-5d20",
  "aud": "https://rls.beta-exchange.example/oauth/token",
  "exp": 1781272253,
  "jti": "394ee516-9c1a-438a-910d-2642c0388049"
}
```

---

**Token response — patient matched (can-spec §6), token bound to Maria**

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "access_token": "mcSB-S1SlvQlQHeFOpmAFTxFqo41Xyn_",
  "token_type": "Bearer",
  "expires_in": 1800,
  "scope": "patient/Patient.rs launch/patient",
  "patient": "beta-master-7741"
}
```

---

**$rls request — parameters are the point of using an operation**

```http
POST https://rls.beta-exchange.example/fhir/Patient/$rls HTTP/1.1
Host: rls.beta-exchange.example
Authorization: Bearer mcSB-S1SlvQlQHeFOpmAFTxFqo41Xyn_
Content-Type: application/fhir+json
```

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "geographic-scope",
      "valueString": "US-CA"
    },
    {
      "name": "since",
      "valueDate": "2020-01-01"
    },
    {
      "name": "resource-interest",
      "valueCode": "Observation"
    }
  ]
}
```

---

**$rls response — endpoints likely to hold Maria's records**

```http
HTTP/1.1 200 OK
Content-Type: application/fhir+json
```

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "location",
      "part": [
        {
          "name": "organization",
          "valueString": "Lakeside Clinic"
        },
        {
          "name": "fhir-endpoint",
          "valueUrl": "https://lakeside.example/fhir"
        }
      ]
    },
    {
      "name": "location",
      "part": [
        {
          "name": "organization",
          "valueString": "County Health"
        },
        {
          "name": "fhir-endpoint",
          "valueUrl": "https://fhir.countyhealth.example/r4"
        }
      ]
    }
  ]
}
```

*Generated 2026-06-12T13:45:53.000Z by `tools/artifact-generator` in this repository. Keys are throwaway examples; every signature verifies against the keys in [keys-and-trust-anchors](keys-and-trust-anchors.md).*