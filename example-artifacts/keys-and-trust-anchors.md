# Keys and trust anchors

*Worked example for [the record location and data access write-up](../authorizing-access.md). Every signature in this library verifies against these keys. All material is throwaway, generated for the examples.*

**CMS statement signing JWKS** (published at a CMS well-known location):

```json
{
  "keys": [
    {
      "crv": "P-384",
      "kty": "EC",
      "x": "1SuTJKvODzv-e-kB7VfisDaEwOL5nQFk76Sk1u8Bm1_cmwLBES5vAm5_bOr32qDT",
      "y": "KvEWHP551xsCmIaSVbJKjvfWbf5qiwo8okqDCQwLv7NNRMztzwoIi7q1PBcyPGSC",
      "alg": "ES384",
      "use": "sig",
      "kid": "z71A5f-J-hex04qi7-XbWFCf17KV1T9ivrubq_chUoM"
    }
  ]
}
```

---

**BP Buddy JWKS** at `https://bpbuddy.example/.well-known/jwks.json` (keys A and B):

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

**CSP (ID.me-style) JWKS:**

```json
{
  "keys": [
    {
      "e": "AQAB",
      "kty": "RSA",
      "n": "s4DJJQ7etPMOr-3k1tZtT-g3bsz9jd9-29simmc5T3plV90bFRD2-zfcmHzDSfSZ2XW8zLlPKRfIxpeJqCSym-EHk4K-Ax9du_pWyW52Y7UsZ8nzktph1fDDQtheh5Qe_T6MsUT5MMtHtJm9mwEyzp422suzVAYwQWKVupCT2SXVzP9b9ctKbVEOmCO_07YvEM0hOwSx010asVwfNvbCyydC2x4E3bW1V5vuSDKXngFrc5zg9sHKjYRoaBxYPrDoJNjJgC2eIZhDf9Jns4f-Drjej4qENDHZIIWCEfF0Ryg5Eq5cL8mSNq_g9ky5PvzApdjjL1-mXTVZs4HSJswzBw",
      "alg": "RS256",
      "use": "sig",
      "kid": "eNu1VtIwGxPVFO8RWvA3T5RjmL2ytO7ZBsGiNJBDPLM"
    }
  ]
}
```

---

**Beta ticket-issuer JWKS** (signs permission tickets in the alternative shape):

```json
{
  "keys": [
    {
      "crv": "P-256",
      "kty": "EC",
      "x": "s-W18XY_6P_Y4VQ-5fT3BgmbIqHZPO3b29f-BryTci0",
      "y": "v6tN2PzQn0QgT-iK5IJdyUbskprlqQeWx58w6I6cMCA",
      "alg": "ES256",
      "use": "sig",
      "kid": "O_XKfVPFibAd-vgESNDuebTVph8MfJouBw2KR3bo_-E"
    }
  ]
}
```

---

**Gamma community CA certificate** (the anchor Gamma distributes to its data holders):

```
-----BEGIN CERTIFICATE-----
MIIDYzCCAkugAwIBAgIULSbILFTzokqU42cLwJvEGKWwgC4wDQYJKoZIhvcNAQEL
BQAwQTEhMB8GA1UEAwwYR2FtbWEgVHJ1c3QgQ29tbXVuaXR5IENBMRwwGgYDVQQK
DBNHYW1tYSBUcnVzdCBOZXR3b3JrMB4XDTI2MDYxMjEzNDU1M1oXDTI3MDYxMjEz
NDU1M1owQTEhMB8GA1UEAwwYR2FtbWEgVHJ1c3QgQ29tbXVuaXR5IENBMRwwGgYD
VQQKDBNHYW1tYSBUcnVzdCBOZXR3b3JrMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
MIIBCgKCAQEAngvWwHdguqzXmHFVvSee+XZtJQzQbYBs4e1qGeNQvSzGBKlkQVvK
wgeAKTyx9oPoPD2UaC3WNWU8Kt9nXJ0n5KMqKtA/12r9kylEEaDKDnjT5pSI83Tm
83kBNnX07oA0eUsIf0E4USE2I5SE0eslIcRAlbkwDeTQWANQtNf3/+4GE5cye9eO
HlqcAIklxij1Rs9AzntZpW5ysLMhx0C4YFkecQHeJSmE7K7/WHjvXuIEVAqb1duX
lRWiZHvivWxwakhL4C33eayAxrIRp5A88NfoHMUzJBv2frBC4b2l+Nk4CZfkF+jd
mEqADWUG3xtYGafVU2wP5JqAOWhj5ONHVwIDAQABo1MwUTAdBgNVHQ4EFgQUhsQV
/QxpRdN0s+OSg9Ol+nbb65owHwYDVR0jBBgwFoAUhsQV/QxpRdN0s+OSg9Ol+nbb
65owDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG9w0BAQsFAAOCAQEAAhGHoRK1mvji
lm9VFl+CJk1WYX0ij/i0Jl+XC1ri6A6Q5yAVYP/Hc6C+/Uik1JUXy6xFUbxI9bkN
nHfpyiEuDQxcTdWwt89nPbp2aWg5Bf+UA6KRigEMvedreY4lVc7ic0ZPaGIpvcGp
0IIPQZPjxxww0pRrKBhd0qFFLTExgytvq/DziCn5Lh4zRrj1KM264jDR8oxMNhMF
UKjL+jbYWFpK3s2uXElVn66lErrKDFTufnZmuCgBOsy0dLMwn3wrpD4bX2GUGZ1P
vUhvJPQpuJYOOxdq1SDE8fvYFI/27PxVMQgnNsBA/MTgQha381RSry8UvdS6N+Fj
7J8FiiQ4Pw==
-----END CERTIFICATE-----
```

---

**BP Buddy's Gamma-issued certificate** (subjectAltName URI = `https://bpbuddy.example`, chains to the CA above):

```
-----BEGIN CERTIFICATE-----
MIIDXzCCAkegAwIBAgIUb/WhDG4LupVf0uCkSK6D1gXi3PgwDQYJKoZIhvcNAQEL
BQAwQTEhMB8GA1UEAwwYR2FtbWEgVHJ1c3QgQ29tbXVuaXR5IENBMRwwGgYDVQQK
DBNHYW1tYSBUcnVzdCBOZXR3b3JrMB4XDTI2MDYxMjEzNDU1M1oXDTI2MDkxMDEz
NDU1M1owKjERMA8GA1UEAwwIQlAgQnVkZHkxFTATBgNVBAoMDEJQIEJ1ZGR5IElu
YzCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANi9HCcWlCfi+1cJ62iG
0PzRfATnljyaWK5BMLns8brkqEDDyrcxdFeLrODDyzUEb+moRlQ2DGRUazzrHo7y
6Dp37QOxRWqnGEERhvsT/dycXpeny88vty65Fmrkn2Qdm6tLxTmm0LH18jSiUcKa
B1jEWd+MdGKFi34tQ4Xqockwx6Y0KSBOoNjg4jvlrJF0+zWlkkHM/FCARQOA5ZDz
HutyreQgJ6TiKcVxnVPACeGtgjprFGSRbPDTa1zMjobXtyMJpfizXewuobQ6popo
iOsjEJGgV8BL6WuiKkJe1vxL39W53zFchERcnGHBUHMCAxcDPvOCl4Yv1J38p2kN
g3MCAwEAAaNmMGQwIgYDVR0RBBswGYYXaHR0cHM6Ly9icGJ1ZGR5LmV4YW1wbGUw
HQYDVR0OBBYEFIcWx+R1Rjpk9sFN4WPlIPxJXeukMB8GA1UdIwQYMBaAFIbEFf0M
aUXTdLPjkoPTpfp22+uaMA0GCSqGSIb3DQEBCwUAA4IBAQAX980hg5ghAYq2CYlC
wxTtWkDIB8s9tpsL8SCyYraH8lwEZY/NDgC6YqK3f8CfnzDc+d6RQdc8d+xldbps
KWRKqoy3qgq1cCkWvA81b3i8q3yTYiAI5uXIIZZDPZM1u7Pcjbn1ZKHWZzCN5Y/T
EJU2t2nz3lTk1s3oAFc5nIUNw1zM9TCQ+7YfZcEgD8wZ6tjO9g6B5p6tQELt14Fw
cKZz2fsr23Ldrjipaj5D7f+fvZ7HfS7vnQUkD1CsRd1VaGK6t26qm9im4cbGeXdC
T8b3xQTdKoE+VXHOFC4RiSqTsSNYqPzBivwKhQzjJ6uzpC0V15klGldc7I4iiVgk
D7xq
-----END CERTIFICATE-----
```

*Generated 2026-06-12T13:45:53.000Z by `tools/artifact-generator` in this repository. Keys are throwaway examples; every signature verifies against the keys in [keys-and-trust-anchors](keys-and-trust-anchors.md).*