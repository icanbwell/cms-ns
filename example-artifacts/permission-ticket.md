# A signed permission ticket and its redemption

*Worked example for [the record location and data access write-up](../authorizing-access.md). The patient authorizes once at a shared authorization service via a SMART App Launch code flow; the token response carries per-site tickets like this one plus endpoint hints (see issuance-token-response). The app redeems the ticket at each data holder's token endpoint via RFC 8693; the data holder verifies the ticket, independently verifies the embedded identity evidence, matches the patient locally, and issues its own token with the matched id.*

**Permission ticket — note subject demographics, the embedded IAL2 id_token as subject_identity_evidence, and the presenter binding to the app's key** (compact JWS, really signed):

```
eyJhbGciOiJFUzI1NiIsImtpZCI6Ik9fWEtmVlBGaWJBZC12Z0VTTkR1ZWJUVnBoOE1mSm91QncyS1IzYm9fLUUiLCJ0eXAiOiJKV1QifQ.eyJ0aWNrZXRfdHlwZSI6InBhdGllbnQtc2VsZi1hY2Nlc3MtdjEiLCJzdWJqZWN0Ijp7InBhdGllbnQiOnsibmFtZSI6W3siZmFtaWx5IjoiTG9wZXoiLCJnaXZlbiI6WyJNYXJpYSJdfV0sImJpcnRoRGF0ZSI6IjE5NjItMDMtMTUifX0sInN1YmplY3RfaWRlbnRpdHlfZXZpZGVuY2UiOiJleUpoYkdjaU9pSlNVekkxTmlJc0ltdHBaQ0k2SW1WT2RURldkRWwzUjNoUVZrWlBPRkpYZGtFelZEVlNhbTFNTW5sMFR6ZGFRbk5IYVU1S1FrUlFURTBpTENKMGVYQWlPaUpLVjFRaWZRLmV5SnBaR1Z1ZEdsMGVWOWhjM04xY21GdVkyVmZiR1YyWld3aU9qSXNJbUYxZEdoZmRHbHRaU0k2TVRjNE1USTNNVGt5TXl3aVoybDJaVzVmYm1GdFpTSTZJazFoY21saElpd2labUZ0YVd4NVgyNWhiV1VpT2lKTWIzQmxlaUlzSW1KcGNuUm9aR0YwWlNJNklqRTVOakl0TURNdE1UVWlMQ0pwYzNNaU9pSm9kSFJ3Y3pvdkwyRndhUzVwWkM1dFpTOXZhV1JqSWl3aWMzVmlJam9pWXpsaFpUYzVabVV0Wm1WbU1TMDBORGxsTFdJMll6a3RZamRrWmpCbFpHSmlOR1V3SWl3aVlYVmtJam9pYUhSMGNITTZMeTlwYzNOMVpYSXVZbVYwWVMxbGVHTm9ZVzVuWlM1bGVHRnRjR3hsSWl3aWFXRjBJam94TnpneE1qY3hPVEl6TENKbGVIQWlPakUzT0RFeU56SXlNak1zSW1wMGFTSTZJbUl5WlRVd01qRXdMVEpsWWpVdE5EWTNNaTFoWVRBMUxURmpNamcwWWprek0yTTVZeUo5LkphaFFaUng2ZnNzTHFjNjBEa0R5V0Z0eUE0QUsybDB2bFY3QWhsTkxRQUJDMUFWeFNpdXB3M3ZYcEFpTG5aZm54WER0ZTQxZWJmazJ5dnZOVEJFZGtHTE9rdHR4T2NXdUxEVjcxZ0ZpeTlpaHY0bzNNd0J6eDZVbXlCV0xON19vNjNNSmdSLXVMY1lldXhnMjk1NUtuYlhmYTRhUEVldWpic21fODRHVGt3b3pLVC1kNW5hU1hfU3VXcXY4b3hBODBaMDM3ZWZtSjN1ZnhOVmFxdTJaZGgyOWZzVXpvNmZGR3g1VXpaSlFZbmNsZHJWR29FRXljM3d4Szh3eXRfMlRDSUZBblpOOUNLZkx4LUFQM0paaEpBc1dUejJRY2hVVktJbHAza0pZOVJlbHJZZXVtTmJpTUVuZWoyT2ZOQy1UMXk0QmNxVWZVd3NJUFM5aVJpRGNKZyIsInByZXNlbnRlcl9iaW5kaW5nIjp7ImprdCI6ImxwRmdBZlZXdzZHWm5tWUtsSzczZDFXUjlSb0hvcWxacWFRY0NiaHRiam8ifSwiYWNjZXNzIjp7InBlcm1pc3Npb25zIjpbeyJyZXNvdXJjZV90eXBlIjoiT2JzZXJ2YXRpb24iLCJpbnRlcmFjdGlvbnMiOlsicmVhZCIsInNlYXJjaCJdfV0sImRhdGFfaG9sZGVyX2ZpbHRlciI6W3sib3JnYW5pemF0aW9uIjoiTGFrZXNpZGUgQ2xpbmljIn1dfSwiaXNzIjoiaHR0cHM6Ly9pc3N1ZXIuYmV0YS1leGNoYW5nZS5leGFtcGxlIiwiYXVkIjoiaHR0cHM6Ly9sYWtlc2lkZS5leGFtcGxlL2ZoaXIiLCJpYXQiOjE3ODEyNzE5NTMsImV4cCI6MTc4MTI3NTU1MywianRpIjoiYmNlNzBlMjItMmZiMC00OWUxLWJiN2YtOGU4MzI0YmFkM2JjIn0.WbZCG0brA_tLT1xYIGPDvGsPhgxu_aHJCCjLLgxiO9VYJT67MwEU5-O9rJQINpYUULGgPzLzaKbXkqIua4tGdw
```

Decoded header:

```json
{
  "alg": "ES256",
  "kid": "O_XKfVPFibAd-vgESNDuebTVph8MfJouBw2KR3bo_-E",
  "typ": "JWT"
}
```

Decoded payload:

```json
{
  "ticket_type": "patient-self-access-v1",
  "subject": {
    "patient": {
      "name": [
        {
          "family": "Lopez",
          "given": [
            "Maria"
          ]
        }
      ],
      "birthDate": "1962-03-15"
    }
  },
  "subject_identity_evidence": "eyJhbGciOiJSUzI1NiIsImtpZCI6ImVOdTFWdEl3R3hQVkZPOFJXdkEzVDVSam1MMnl0TzdaQnNHaU5KQkRQTE0iLCJ0eXAiOiJKV1QifQ.eyJpZGVudGl0eV9hc3N1cmFuY2VfbGV2ZWwiOjIsImF1dGhfdGltZSI6MTc4MTI3MTkyMywiZ2l2ZW5fbmFtZSI6Ik1hcmlhIiwiZmFtaWx5X25hbWUiOiJMb3BleiIsImJpcnRoZGF0ZSI6IjE5NjItMDMtMTUiLCJpc3MiOiJodHRwczovL2FwaS5pZC5tZS9vaWRjIiwic3ViIjoiYzlhZTc5ZmUtZmVmMS00NDllLWI2YzktYjdkZjBlZGJiNGUwIiwiYXVkIjoiaHR0cHM6Ly9pc3N1ZXIuYmV0YS1leGNoYW5nZS5leGFtcGxlIiwiaWF0IjoxNzgxMjcxOTIzLCJleHAiOjE3ODEyNzIyMjMsImp0aSI6ImIyZTUwMjEwLTJlYjUtNDY3Mi1hYTA1LTFjMjg0YjkzM2M5YyJ9.JahQZRx6fssLqc60DkDyWFtyA4AK2l0vlV7AhlNLQABC1AVxSiupw3vXpAiLnZfnxXDte41ebfk2yvvNTBEdkGLOkttxOcWuLDV71gFiy9ihv4o3MwBzx6UmyBWLN7_o63MJgR-uLcYeuxg2955KnbXfa4aPEeujbsm_84GTkwozKT-d5naSX_SuWqv8oxA80Z037efmJ3ufxNVaqu2Zdh29fsUzo6fFGx5UzZJQYncldrVGoEEyc3wxK8wyt_2TCIFAnZN9CKfLx-AP3JZhJAsWTz2QchUVKIlp3kJY9RelrYeumNbiMEnej2OfNC-T1y4BcqUfUwsIPS9iRiDcJg",
  "presenter_binding": {
    "jkt": "lpFgAfVWw6GZnmYKlK73d1WR9RoHoqlZqaQcCbhtbjo"
  },
  "access": {
    "permissions": [
      {
        "resource_type": "Observation",
        "interactions": [
          "read",
          "search"
        ]
      }
    ],
    "data_holder_filter": [
      {
        "organization": "Lakeside Clinic"
      }
    ]
  },
  "iss": "https://issuer.beta-exchange.example",
  "aud": "https://lakeside.example/fhir",
  "iat": 1781271953,
  "exp": 1781275553,
  "jti": "bce70e22-2fb0-49e1-bb7f-8e8324bad3bc"
}
```

The `subject_identity_evidence` is a CSP-signed id_token whose `aud` names the ticket issuer: the issuer ran the CSP sign-in as relying party during the issuance ceremony. The data holder verifies the evidence's signature against the CSP's keys itself (evidence-issuer trust is configured separately from ticket-issuer trust) and resolves the evidence's client identifier to the ticket issuer. `presenter_binding.jkt` is the thumbprint of the app key in [keys-and-trust-anchors](keys-and-trust-anchors.md).

---

**Redemption — RFC 8693 token exchange at the data holder**

```http
POST https://lakeside.example/oauth/token HTTP/1.1
Host: lakeside.example
Content-Type: application/x-www-form-urlencoded
```

```json
{
  "grant_type": "urn:ietf:params:oauth:grant-type:token-exchange",
  "subject_token_type": "https://smarthealthit.org/token-type/permission-ticket",
  "subject_token": "eyJhbGciOiJFUzI1NiIsImtpZCI6Ik9fWEtmVlBGaWJBZC12Z0VTTkR1ZWJU... (full value above)",
  "scope": "patient/Observation.rs",
  "client_assertion_type": "urn:ietf:params:oauth:client-assertion-type:jwt-bearer",
  "client_assertion": "(signed with the key named by presenter_binding.jkt)"
}
```

---

**Token response — the data holder's own token, with its matched patient id**

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "access_token": "(bearer token used below)",
  "token_type": "Bearer",
  "expires_in": 3600,
  "scope": "patient/Observation.rs",
  "patient": "lakeside-449210"
}
```

---

**FHIR query with that token**

```http
GET https://lakeside.example/fhir/Observation?patient=lakeside-449210&category=vital-signs&_count=1 HTTP/1.1
Authorization: Bearer (token from above)
Accept: application/fhir+json
```

---

**FHIR response**

```http
HTTP/1.1 200 OK
Content-Type: application/fhir+json
```

```json
{
  "resourceType": "Bundle",
  "type": "searchset",
  "total": 1,
  "entry": [
    {
      "resource": {
        "resourceType": "Observation",
        "status": "final",
        "code": {
          "coding": [
            {
              "system": "http://loinc.org",
              "code": "85354-9",
              "display": "Blood pressure panel"
            }
          ]
        },
        "subject": {
          "reference": "Patient/lakeside-449210"
        },
        "effectiveDateTime": "2026-06-02T14:10:00Z",
        "component": [
          {
            "code": {
              "coding": [
                {
                  "system": "http://loinc.org",
                  "code": "8480-6"
                }
              ]
            },
            "valueQuantity": {
              "value": 131,
              "unit": "mmHg"
            }
          },
          {
            "code": {
              "coding": [
                {
                  "system": "http://loinc.org",
                  "code": "8462-4"
                }
              ]
            },
            "valueQuantity": {
              "value": 82,
              "unit": "mmHg"
            }
          }
        ]
      }
    }
  ]
}
```

*Generated 2026-06-12T13:45:53.000Z by `tools/artifact-generator` in this repository. Keys are throwaway examples; every signature verifies against the keys in [keys-and-trust-anchors](keys-and-trust-anchors.md).*