# A blanket ticket: every match disclosed

*Worked example for [the record location and data access write-up](../authorizing-access.md). If Maria chooses every site (or the deployment does in-app selection), the token response carries a single ticket with no data_holder_filter, and endpoint hints for every match. The app learns every care relationship; this is the disclosure that service-side selection avoids.*

**Blanket ticket: no data_holder_filter** (compact JWS, really signed):

```
eyJhbGciOiJFUzI1NiIsImtpZCI6Ik9fWEtmVlBGaWJBZC12Z0VTTkR1ZWJUVnBoOE1mSm91QncyS1IzYm9fLUUiLCJ0eXAiOiJKV1QifQ.eyJ0aWNrZXRfdHlwZSI6InBhdGllbnQtc2VsZi1hY2Nlc3MtdjEiLCJzdWJqZWN0Ijp7InBhdGllbnQiOnsibmFtZSI6W3siZmFtaWx5IjoiTG9wZXoiLCJnaXZlbiI6WyJNYXJpYSJdfV0sImJpcnRoRGF0ZSI6IjE5NjItMDMtMTUifX0sInN1YmplY3RfaWRlbnRpdHlfZXZpZGVuY2UiOiJleUpoYkdjaU9pSlNVekkxTmlJc0ltdHBaQ0k2SW1WT2RURldkRWwzUjNoUVZrWlBPRkpYZGtFelZEVlNhbTFNTW5sMFR6ZGFRbk5IYVU1S1FrUlFURTBpTENKMGVYQWlPaUpLVjFRaWZRLmV5SnBaR1Z1ZEdsMGVWOWhjM04xY21GdVkyVmZiR1YyWld3aU9qSXNJbUYxZEdoZmRHbHRaU0k2TVRjNE1USTNNVGt5TXl3aVoybDJaVzVmYm1GdFpTSTZJazFoY21saElpd2labUZ0YVd4NVgyNWhiV1VpT2lKTWIzQmxlaUlzSW1KcGNuUm9aR0YwWlNJNklqRTVOakl0TURNdE1UVWlMQ0pwYzNNaU9pSm9kSFJ3Y3pvdkwyRndhUzVwWkM1dFpTOXZhV1JqSWl3aWMzVmlJam9pWXpsaFpUYzVabVV0Wm1WbU1TMDBORGxsTFdJMll6a3RZamRrWmpCbFpHSmlOR1V3SWl3aVlYVmtJam9pYUhSMGNITTZMeTlwYzNOMVpYSXVZbVYwWVMxbGVHTm9ZVzVuWlM1bGVHRnRjR3hsSWl3aWFXRjBJam94TnpneE1qY3hPVEl6TENKbGVIQWlPakUzT0RFeU56SXlNak1zSW1wMGFTSTZJbUl5WlRVd01qRXdMVEpsWWpVdE5EWTNNaTFoWVRBMUxURmpNamcwWWprek0yTTVZeUo5LkphaFFaUng2ZnNzTHFjNjBEa0R5V0Z0eUE0QUsybDB2bFY3QWhsTkxRQUJDMUFWeFNpdXB3M3ZYcEFpTG5aZm54WER0ZTQxZWJmazJ5dnZOVEJFZGtHTE9rdHR4T2NXdUxEVjcxZ0ZpeTlpaHY0bzNNd0J6eDZVbXlCV0xON19vNjNNSmdSLXVMY1lldXhnMjk1NUtuYlhmYTRhUEVldWpic21fODRHVGt3b3pLVC1kNW5hU1hfU3VXcXY4b3hBODBaMDM3ZWZtSjN1ZnhOVmFxdTJaZGgyOWZzVXpvNmZGR3g1VXpaSlFZbmNsZHJWR29FRXljM3d4Szh3eXRfMlRDSUZBblpOOUNLZkx4LUFQM0paaEpBc1dUejJRY2hVVktJbHAza0pZOVJlbHJZZXVtTmJpTUVuZWoyT2ZOQy1UMXk0QmNxVWZVd3NJUFM5aVJpRGNKZyIsInByZXNlbnRlcl9iaW5kaW5nIjp7ImprdCI6ImxwRmdBZlZXdzZHWm5tWUtsSzczZDFXUjlSb0hvcWxacWFRY0NiaHRiam8ifSwiYWNjZXNzIjp7InBlcm1pc3Npb25zIjpbeyJyZXNvdXJjZV90eXBlIjoiT2JzZXJ2YXRpb24iLCJpbnRlcmFjdGlvbnMiOlsicmVhZCIsInNlYXJjaCJdfV19LCJpc3MiOiJodHRwczovL2lzc3Vlci5iZXRhLWV4Y2hhbmdlLmV4YW1wbGUiLCJhdWQiOiJodHRwczovL2JldGEtZXhjaGFuZ2UuZXhhbXBsZS9kYXRhLWhvbGRlcnMiLCJpYXQiOjE3ODEyNzE5NTMsImV4cCI6MTc4MTI3NTU1MywianRpIjoiNDYyNGNiMDgtYTAyNS00M2NlLTk1ZWYtMzViZTdmYTdhMmQ4In0.84lQ-DQo150OMCTrenZH-sFWjl6O1_KGFRWxT6EP1qIomu0HsgEEHXr_i8_iSqtaqTNbUKZphH3U2Q54qc7Gsw
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
    ]
  },
  "iss": "https://issuer.beta-exchange.example",
  "aud": "https://beta-exchange.example/data-holders",
  "iat": 1781271953,
  "exp": 1781275553,
  "jti": "4624cb08-a025-43ce-95ef-35be7fa7a2d8"
}
```

---

**Endpoint hints accompanying it: every match, all pointing at ticket 0**

```http
HTTP/1.1 200 OK (excerpt)
```

```json
{
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
        0
      ]
    },
    {
      "fhir_base_url": "https://fhir.generalhospital.example/r4",
      "organization": {
        "resourceType": "Organization",
        "name": "General Hospital"
      },
      "ticket_indices": [
        0
      ]
    }
  ]
}
```

*Generated 2026-06-12T13:45:53.000Z by `tools/artifact-generator` in this repository. Keys are throwaway examples; every signature verifies against the keys in [keys-and-trust-anchors](keys-and-trust-anchors.md).*