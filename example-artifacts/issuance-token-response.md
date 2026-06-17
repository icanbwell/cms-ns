# The authorization step's token response

*Worked example for [the record location and data access write-up](../authorizing-access.md). What BP Buddy receives when Maria finishes the authorization step at the shared authorization service: a standard SMART token response extended with per-site permission tickets and endpoint hints. Maria chose two sites; sites she left out appear nowhere.*

**Token response — authorization code exchanged at the service's token endpoint**

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "access_token": "2ogdPBEZLMTB1xLteg0PWZpezN2ijqUS",
  "token_type": "Bearer",
  "expires_in": 300,
  "refresh_token": "REKHUYBQK4AM6mF7vyAHU6DqOcUMGmYI",
  "scope": "permission_ticket patient/Observation.rs offline_access",
  "smart_permission_ticket": [
    "eyJhbGciOiJFUzI1NiIsImtpZCI6Ik9fWEtmVlBGaWJBZC12Z0... (ticket 0, decoded below)",
    "eyJhbGciOiJFUzI1NiIsImtpZCI6Ik9fWEtmVlBGaWJBZC12Z0... (ticket 1, decoded below)"
  ],
  "smart_permission_ticket_endpoints": [
    {
      "fhir_base_url": "https://lakeside.example/fhir",
      "organization": {
        "resourceType": "Organization",
        "name": "Lakeside Clinic"
      },
      "ticket_indices": [
        0
      ]
    },
    {
      "fhir_base_url": "https://fhir.countyhealth.example/r4",
      "organization": {
        "resourceType": "Organization",
        "name": "County Health"
      },
      "ticket_indices": [
        1
      ]
    }
  ]
}
```

---

**Ticket 0 — scoped to Lakeside Clinic** (compact JWS, really signed):

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

---

**Ticket 1 — scoped to County Health** (compact JWS, really signed):

```
eyJhbGciOiJFUzI1NiIsImtpZCI6Ik9fWEtmVlBGaWJBZC12Z0VTTkR1ZWJUVnBoOE1mSm91QncyS1IzYm9fLUUiLCJ0eXAiOiJKV1QifQ.eyJ0aWNrZXRfdHlwZSI6InBhdGllbnQtc2VsZi1hY2Nlc3MtdjEiLCJzdWJqZWN0Ijp7InBhdGllbnQiOnsibmFtZSI6W3siZmFtaWx5IjoiTG9wZXoiLCJnaXZlbiI6WyJNYXJpYSJdfV0sImJpcnRoRGF0ZSI6IjE5NjItMDMtMTUifX0sInN1YmplY3RfaWRlbnRpdHlfZXZpZGVuY2UiOiJleUpoYkdjaU9pSlNVekkxTmlJc0ltdHBaQ0k2SW1WT2RURldkRWwzUjNoUVZrWlBPRkpYZGtFelZEVlNhbTFNTW5sMFR6ZGFRbk5IYVU1S1FrUlFURTBpTENKMGVYQWlPaUpLVjFRaWZRLmV5SnBaR1Z1ZEdsMGVWOWhjM04xY21GdVkyVmZiR1YyWld3aU9qSXNJbUYxZEdoZmRHbHRaU0k2TVRjNE1USTNNVGt5TXl3aVoybDJaVzVmYm1GdFpTSTZJazFoY21saElpd2labUZ0YVd4NVgyNWhiV1VpT2lKTWIzQmxlaUlzSW1KcGNuUm9aR0YwWlNJNklqRTVOakl0TURNdE1UVWlMQ0pwYzNNaU9pSm9kSFJ3Y3pvdkwyRndhUzVwWkM1dFpTOXZhV1JqSWl3aWMzVmlJam9pWXpsaFpUYzVabVV0Wm1WbU1TMDBORGxsTFdJMll6a3RZamRrWmpCbFpHSmlOR1V3SWl3aVlYVmtJam9pYUhSMGNITTZMeTlwYzNOMVpYSXVZbVYwWVMxbGVHTm9ZVzVuWlM1bGVHRnRjR3hsSWl3aWFXRjBJam94TnpneE1qY3hPVEl6TENKbGVIQWlPakUzT0RFeU56SXlNak1zSW1wMGFTSTZJbUl5WlRVd01qRXdMVEpsWWpVdE5EWTNNaTFoWVRBMUxURmpNamcwWWprek0yTTVZeUo5LkphaFFaUng2ZnNzTHFjNjBEa0R5V0Z0eUE0QUsybDB2bFY3QWhsTkxRQUJDMUFWeFNpdXB3M3ZYcEFpTG5aZm54WER0ZTQxZWJmazJ5dnZOVEJFZGtHTE9rdHR4T2NXdUxEVjcxZ0ZpeTlpaHY0bzNNd0J6eDZVbXlCV0xON19vNjNNSmdSLXVMY1lldXhnMjk1NUtuYlhmYTRhUEVldWpic21fODRHVGt3b3pLVC1kNW5hU1hfU3VXcXY4b3hBODBaMDM3ZWZtSjN1ZnhOVmFxdTJaZGgyOWZzVXpvNmZGR3g1VXpaSlFZbmNsZHJWR29FRXljM3d4Szh3eXRfMlRDSUZBblpOOUNLZkx4LUFQM0paaEpBc1dUejJRY2hVVktJbHAza0pZOVJlbHJZZXVtTmJpTUVuZWoyT2ZOQy1UMXk0QmNxVWZVd3NJUFM5aVJpRGNKZyIsInByZXNlbnRlcl9iaW5kaW5nIjp7ImprdCI6ImxwRmdBZlZXdzZHWm5tWUtsSzczZDFXUjlSb0hvcWxacWFRY0NiaHRiam8ifSwiYWNjZXNzIjp7InBlcm1pc3Npb25zIjpbeyJyZXNvdXJjZV90eXBlIjoiT2JzZXJ2YXRpb24iLCJpbnRlcmFjdGlvbnMiOlsicmVhZCIsInNlYXJjaCJdfV0sImRhdGFfaG9sZGVyX2ZpbHRlciI6W3sib3JnYW5pemF0aW9uIjoiQ291bnR5IEhlYWx0aCJ9XX0sImlzcyI6Imh0dHBzOi8vaXNzdWVyLmJldGEtZXhjaGFuZ2UuZXhhbXBsZSIsImF1ZCI6Imh0dHBzOi8vZmhpci5jb3VudHloZWFsdGguZXhhbXBsZS9yNCIsImlhdCI6MTc4MTI3MTk1MywiZXhwIjoxNzgxMjc1NTUzLCJqdGkiOiI1ODUxM2ZhMi1jMmMzLTQ4MGItOTAzMi1jZjY0MmE3NzhhNTUifQ.Ft_1nj5dXJU30nxWYByNNztykNGzpFZojBOylwJQXTaaszV_Q9NPYdHTCaLF0c0equpSzBw7Ows3wsj_aB6wqA
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
        "organization": "County Health"
      }
    ]
  },
  "iss": "https://issuer.beta-exchange.example",
  "aud": "https://fhir.countyhealth.example/r4",
  "iat": 1781271953,
  "exp": 1781275553,
  "jti": "58513fa2-c2c3-480b-9032-cf642a778a55"
}
```

---

**Renewing tickets later: the refresh_token re-mints them without re-running the authorization step**

```http
POST https://issuer.beta-exchange.example/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
```

```json
{
  "grant_type": "refresh_token",
  "refresh_token": "(value from the response above)",
  "client_id": "sas-bp-buddy-3f81"
}
```

---

The refresh response has the same shape as the original: a fresh smart_permission_ticket array for the same site selection, with new expirations.

---

If Maria instead chooses every site in the network (the alternative where the app sees every match), the response carries one blanket ticket with no data_holder_filter and endpoint hints for every match: see [blanket-ticket](blanket-ticket.md). Redeeming a per-site ticket at a data holder is shown in [permission-ticket](permission-ticket.md).

*Generated 2026-06-12T13:45:53.000Z by `tools/artifact-generator` in this repository. Keys are throwaway examples; every signature verifies against the keys in [keys-and-trust-anchors](keys-and-trust-anchors.md).*