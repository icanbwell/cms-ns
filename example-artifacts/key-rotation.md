# Key rotation

*Worked example for [the record location and data access write-up](../authorizing-access.md). The app publishes key B alongside key A, then signs with the new kid; data holders resolve it at the live jwks_uri with nothing to re-issue.*

**JWKS before rotation:**

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

**JWKS during the overlap window (key B published alongside key A):**

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
    },
    {
      "e": "AQAB",
      "kty": "RSA",
      "n": "yXvrbQbgjmLeuJmLMSBB-W3TAXONQDqlgddfRmPMbkRq4qF7KgMVuY06BxL26de-OoWDpEn3Uo39JOncH5oMCApBOGW_inYBGdOHfJhc0lSOxnXmLfpfBRc1eodVu_-WA6MeC2mrXdCiQvFj-i8yvRlW-ikdgP9s1zP-ZCK5M3utDJmJfAU_k4P1BEJxPNLlHvPdR_lXZEJDh4tVSYYmfqRKlpPK4JKwrnIFA9njyNZujuR7snMVLMWWK_jZ-0oqrEy-XlTcQKrfKjax1Goeqk-nzdf8jdZqwtWL4BEI7LmT5QOtPza2mkTJ8adNqCBiIwzimbd1cH4YG1t8L1RJcQ",
      "alg": "RS384",
      "use": "sig",
      "kid": "T49_AjrMZbFCbrU6EuBMaStZ23Ega_K0_EJ1Sy4bRWc"
    }
  ]
}
```

---

First token request signed with the new key; note the `kid` in the header now matches key B:

**client_assertion signed with key B** (compact JWS, really signed):

```
eyJhbGciOiJSUzM4NCIsImtpZCI6IlQ0OV9BanJNWmJGQ2JyVTZFdUJNYVN0WjIzRWdhX0swX0VKMVN5NGJSV2MiLCJ0eXAiOiJKV1QifQ.eyJleHRlbnNpb25zIjp7ImNtc19zbWFydCI6eyJ2ZXJzaW9uIjoiMSIsInB1cnBvc2Vfb2ZfdXNlIjoiUEFUUlFUIiwiaWRfdG9rZW4iOiJleUpoYkdjaU9pSlNVekkxTmlJc0ltdHBaQ0k2SW1WT2RURldkRWwzUjNoUVZrWlBPRkpYZGtFelZEVlNhbTFNTW5sMFR6ZGFRbk5IYVU1S1FrUlFURTBpTENKMGVYQWlPaUpLVjFRaWZRLmV5SnBaR1Z1ZEdsMGVWOWhjM04xY21GdVkyVmZiR1YyWld3aU9qSXNJbUYxZEdoZmRHbHRaU0k2TVRjNE1USTNNVGc1TXl3aVoybDJaVzVmYm1GdFpTSTZJazFoY21saElpd2labUZ0YVd4NVgyNWhiV1VpT2lKTWIzQmxlaUlzSW1KcGNuUm9aR0YwWlNJNklqRTVOakl0TURNdE1UVWlMQ0poWkdSeVpYTnpJanA3SW5OMGNtVmxkRjloWkdSeVpYTnpJam9pTkRFNElFRnNaR1Z5SUVOdmRYSjBJaXdpYkc5allXeHBkSGtpT2lKU2FYWmxjbk5wWkdVaUxDSnlaV2RwYjI0aU9pSkRRU0lzSW5CdmMzUmhiRjlqYjJSbElqb2lPVEkxTURFaUxDSmpiM1Z1ZEhKNUlqb2lWVk1pZlN3aWMzTnVYMmwwYVc1ZmMyaHZjblFpT2lJME16SXhJaXdpYVhOeklqb2lhSFIwY0hNNkx5OWhjR2t1YVdRdWJXVXZiMmxrWXlJc0luTjFZaUk2SWpreU16ZGxOalJoTFdVMk1tRXRORFZrTVMwNFpHSXhMVFZqTURNeE1XRTFNekl3WXlJc0ltRjFaQ0k2SW1oMGRIQnpPaTh2YkdsaWNtRnllUzV0WldScFkyRnlaUzVuYjNZdllYQndMV3hwWW5KaGNua3ZZWEJ3Y3k5aWNDMWlkV1JrZVNJc0ltbGhkQ0k2TVRjNE1USTNNVGc1TXl3aVpYaHdJam94TnpneE1qY3lNVGt6TENKcWRHa2lPaUkxWVdFMVltRmlOQzAyWkRnekxUUXhOVFl0T0RZeFpTMHhaRE0zTldJNVpXUTROV01pZlEuZDJIQ0tNdFVoOFJLaFBiSmQzT3ZxLWlwc3N4anI4eG4wOVl6NE1MOE9LMnFDRVdSeUNIV2c0akNCRnJvTFI5OXFNc3l5TmhleFRSVUUtMjh4dEUtMC1Qc2czQTlObUtNVXd5WXFQV2t1bFZFNGhUQUxSX1lSTmIzUXhmYzJJNUYxUFNjSXY5N2J1RGNoUEdkUDlIVHVOZmk1OHBPcEZlY3pidjE0bEphZlFxNGJWQlE4NHloNEZqdUlfTENmU1VYQzVLeXVIS285dnpIZXlOWVdyMXlHWERDaWg4QmdHODJtY1FvaTVBcHo3N2pJUlp3V282MWNLUmt1SEpuTk1saDU3UjIwcVhjeWZZRXBBa2ZLUW5Zd0tpcno4UEZoYXpPcDJYZDBMZFJUaENQNV9mZFdQWmlBV0dJMzRWSVpIRE9XWWFBeXBKWjl6YlgxdTdXZTJnNHdnIn19LCJpc3MiOiJsYWtlc2lkZS1kaC1icC1idWRkeS05MWFmIiwic3ViIjoibGFrZXNpZGUtZGgtYnAtYnVkZHktOTFhZiIsImF1ZCI6Imh0dHBzOi8vbGFrZXNpZGUuZXhhbXBsZS9vYXV0aC90b2tlbiIsImV4cCI6MTc4MTI3MjI1MywianRpIjoiZjc5ZmJlYmQtMDMxOC00YmQ3LThkMmItOGMwMTMyNzVkNTliIn0.xjD0HRKbEEsG4GGEuD0mW8UcbDEOj5M1Hh84wCRZfy5AeG7oMiJ_m6Belct65f8hS8g_BXLfr9Ni1ZDe_f501aE2n72L4gKveAwiZj5PGFcWHTMB09VOaYdZVpULTYc1-O3Al8AKpVqSMO7GPr5fITTleA94vGt1JK5RbeUP320ggFjCWMfY3GAdgfl1i-EVHaMo_LzKg6AJY_Pg50Ot_XyFZlzNfvAHfwjQz0lzWzqtCKmjZRM7spKo5htGmDbSh904BaiXEEv4Sg_OSC-nfXOFrexKIAGMuuSx0xCM9f1srLBEhDhZQmo9J_nShnqZdPiiC4-PRDEBc12jHg1pUQ
```

Decoded header:

```json
{
  "alg": "RS384",
  "kid": "T49_AjrMZbFCbrU6EuBMaStZ23Ega_K0_EJ1Sy4bRWc",
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
  "jti": "f79fbebd-0318-4bd7-8d2b-8c013275d59b"
}
```

---

After the overlap window the app removes key A from the JWKS. Nothing else in the ecosystem changed: the CMS statement binds the `jwks_uri`, not a key. For network-issued certificates, the synchronization rule in [Key rotation](../authorizing-access.md) applies.

*Generated 2026-06-12T13:45:53.000Z by `tools/artifact-generator` in this repository. Keys are throwaway examples; every signature verifies against the keys in [keys-and-trust-anchors](keys-and-trust-anchors.md).*