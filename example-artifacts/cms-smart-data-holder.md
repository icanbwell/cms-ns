# cms_smart at a data holder: token and FHIR retrieval

*Worked example for [the record location and data access write-up](../authorizing-access.md). Same token shape as everywhere else; the only difference from 4a is that the data holder's own authorization server issues the token, and a refresh_token supports the rolling 90-day window of can-spec §9.*

**Token request**

```http
POST https://lakeside.example/oauth/token HTTP/1.1
Host: lakeside.example
Content-Type: application/x-www-form-urlencoded
```

```json
{
  "grant_type": "client_credentials",
  "scope": "patient/Observation.rs patient/MedicationRequest.rs launch/patient",
  "client_assertion_type": "urn:ietf:params:oauth:client-assertion-type:jwt-bearer",
  "client_assertion": "eyJhbGciOiJSUzM4NCIsImtpZCI6ImxwRmdBZlZXdzZHWm5tWUtsSzczZDFX... (decoded below)"
}
```

---

**client_assertion** (compact JWS, really signed):

```
eyJhbGciOiJSUzM4NCIsImtpZCI6ImxwRmdBZlZXdzZHWm5tWUtsSzczZDFXUjlSb0hvcWxacWFRY0NiaHRiam8iLCJ0eXAiOiJKV1QifQ.eyJleHRlbnNpb25zIjp7ImNtc19zbWFydCI6eyJ2ZXJzaW9uIjoiMSIsInB1cnBvc2Vfb2ZfdXNlIjoiUEFUUlFUIiwiaWRfdG9rZW4iOiJleUpoYkdjaU9pSlNVekkxTmlJc0ltdHBaQ0k2SW1WT2RURldkRWwzUjNoUVZrWlBPRkpYZGtFelZEVlNhbTFNTW5sMFR6ZGFRbk5IYVU1S1FrUlFURTBpTENKMGVYQWlPaUpLVjFRaWZRLmV5SnBaR1Z1ZEdsMGVWOWhjM04xY21GdVkyVmZiR1YyWld3aU9qSXNJbUYxZEdoZmRHbHRaU0k2TVRjNE1USTNNVGc1TXl3aVoybDJaVzVmYm1GdFpTSTZJazFoY21saElpd2labUZ0YVd4NVgyNWhiV1VpT2lKTWIzQmxlaUlzSW1KcGNuUm9aR0YwWlNJNklqRTVOakl0TURNdE1UVWlMQ0poWkdSeVpYTnpJanA3SW5OMGNtVmxkRjloWkdSeVpYTnpJam9pTkRFNElFRnNaR1Z5SUVOdmRYSjBJaXdpYkc5allXeHBkSGtpT2lKU2FYWmxjbk5wWkdVaUxDSnlaV2RwYjI0aU9pSkRRU0lzSW5CdmMzUmhiRjlqYjJSbElqb2lPVEkxTURFaUxDSmpiM1Z1ZEhKNUlqb2lWVk1pZlN3aWMzTnVYMmwwYVc1ZmMyaHZjblFpT2lJME16SXhJaXdpYVhOeklqb2lhSFIwY0hNNkx5OWhjR2t1YVdRdWJXVXZiMmxrWXlJc0luTjFZaUk2SWpreU16ZGxOalJoTFdVMk1tRXRORFZrTVMwNFpHSXhMVFZqTURNeE1XRTFNekl3WXlJc0ltRjFaQ0k2SW1oMGRIQnpPaTh2YkdsaWNtRnllUzV0WldScFkyRnlaUzVuYjNZdllYQndMV3hwWW5KaGNua3ZZWEJ3Y3k5aWNDMWlkV1JrZVNJc0ltbGhkQ0k2TVRjNE1USTNNVGc1TXl3aVpYaHdJam94TnpneE1qY3lNVGt6TENKcWRHa2lPaUkxWVdFMVltRmlOQzAyWkRnekxUUXhOVFl0T0RZeFpTMHhaRE0zTldJNVpXUTROV01pZlEuZDJIQ0tNdFVoOFJLaFBiSmQzT3ZxLWlwc3N4anI4eG4wOVl6NE1MOE9LMnFDRVdSeUNIV2c0akNCRnJvTFI5OXFNc3l5TmhleFRSVUUtMjh4dEUtMC1Qc2czQTlObUtNVXd5WXFQV2t1bFZFNGhUQUxSX1lSTmIzUXhmYzJJNUYxUFNjSXY5N2J1RGNoUEdkUDlIVHVOZmk1OHBPcEZlY3pidjE0bEphZlFxNGJWQlE4NHloNEZqdUlfTENmU1VYQzVLeXVIS285dnpIZXlOWVdyMXlHWERDaWg4QmdHODJtY1FvaTVBcHo3N2pJUlp3V282MWNLUmt1SEpuTk1saDU3UjIwcVhjeWZZRXBBa2ZLUW5Zd0tpcno4UEZoYXpPcDJYZDBMZFJUaENQNV9mZFdQWmlBV0dJMzRWSVpIRE9XWWFBeXBKWjl6YlgxdTdXZTJnNHdnIn19LCJpc3MiOiJsYWtlc2lkZS1kaC1icC1idWRkeS05MWFmIiwic3ViIjoibGFrZXNpZGUtZGgtYnAtYnVkZHktOTFhZiIsImF1ZCI6Imh0dHBzOi8vbGFrZXNpZGUuZXhhbXBsZS9vYXV0aC90b2tlbiIsImV4cCI6MTc4MTI3MjI1MywianRpIjoiYjI3YWE4MzgtN2I2OS00OTdjLTkyYTMtYzJlOWI1NDUzYzliIn0.B1M7N3CP4FQ7DFQwBIbvja21uzgYSTVey1zH9pNgKg7jvDjOZ2MqRZ8wZqz5OEvBtC1_fjvG_-m7KngfIr5XeqdkaPt552TXPBWzX2Yj65IKAnNRV33Ua2-0bJxwfA1bV6ni5wq7xc9qIjUojZnC9MBogV39kQKjfdKjVcGIK6jug-EwlHICTQz89pNEov7HBYelml29mnpHzgahadG5LTu4ssIIoqZXunsRUXiJ-FuOVdl71qJEdNpUOTCBQDRs2H5OuWHdMMufAI5xorFQuw6YQo6z8m4RLdcQBQULPgAvfDuTU7-R7Se34DZNJZvrESPzsYgjk13VnvrGcNYvww
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
  "iss": "lakeside-dh-bp-buddy-91af",
  "sub": "lakeside-dh-bp-buddy-91af",
  "aud": "https://lakeside.example/oauth/token",
  "exp": 1781272253,
  "jti": "b27aa838-7b69-497c-92a3-c2e9b5453c9b"
}
```

---

**Token response**

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "access_token": "CyXjkEh8EAsj5vMJKL3yOXeD-HACf1ZA",
  "refresh_token": "cHbYWc4f6__qRupwddkc-uvaw9DVswU8",
  "token_type": "Bearer",
  "expires_in": 3600,
  "scope": "patient/Observation.rs patient/MedicationRequest.rs launch/patient",
  "patient": "lakeside-449210"
}
```

---

**FHIR query**

```http
GET https://lakeside.example/fhir/Observation?patient=lakeside-449210&category=vital-signs&_count=1 HTTP/1.1
Authorization: Bearer CyXjkEh8EAsj5vMJKL3yOXeD-HACf1ZA
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