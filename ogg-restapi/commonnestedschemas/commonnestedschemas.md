# Appendix: Common Nested Schemas

### A1: Credential Reference
```json
{
  "$schema": "ogg:credentialsRef",
  "domain": "OracleGoldenGate",
  "alias": "<aliasName>"
}
```

---

### Begin Variants

```json
// Variant 1 -- Start from now
"begin": "now"

// Variant 2 -- Start from ISO-8601 timestamp
"begin": "2025-06-01T00:00:00.000Z"

// Variant 3 -- Oracle SCN (integer)
"begin": {
  "$schema": "type:position/atDbms",
  "at": {
    "csn": 6488359
  }
}

// Variant 4 -- Trail RBA position (Replicat/Distribution)
"begin": {
  "$schema": "type:position/atTrailRBA",
  "sequence": 100,
  "offset": 4096
}
```

---

### Managed Process Settings

```json
{
  "$schema": "ogg:managedProcessSettings",
  "autoStart": { "enabled": true, "delay": 0 },
  "autoRestart": {
    "enabled": true,
    "delay": 30,
    "retries": 9,
    "window": 60,
    "disableOnFailure": true
  }
}
```

---

## OCI Archive Target Authentication Variants

```
// Variant 1 -- Profile
"authentication": {
  "profile": "DEFAULT"
}

// Variant 2 -- Instance Principal
"authentication": {
  "instancePrincipal": true
}

// Variant 3 -- API Signing Key
"authentication": {
  "tenancyId": "ocid1.tenancy.oc1...",
  "userId": "ocid1.user.oc1...",
  "apiSigningKey": "-----BEGIN RSA PRIVATE KEY-----\n...\n-----END RSA PRIVATE KEY-----",
  "apiSigningKeyFingerprint": "aa:bb:cc:dd:ee:ff:00:11:22:33:44:55:66:77:88:99"
}

```
---

## S3-Compatible Archive Target

```
{
  "bucketName": "my-gg-archive-bucket",
  "region": "us-east-1",
  "authentication": {
    "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
    "secretKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
  }
}

```

---

### Standard API Response

All write operations return a standard response envelope:

```json
{
  "$schema": "api:standardResponse",
  "response": { /* created/updated resource object */ },
  "messages": [
    {
      "$schema": "ogg:message",
      "type": "https://goldengate.oracle.com/messages/OGG-00001",
      "title": "Process created successfully.",
      "code": "OGG-00001",
      "severity": "INFO",
      "issued": "2026-04-02T10:30:00.000Z"
    }
  ],
  "links": [
    {
      "$schema": "ogg:link",
      "rel": "self",
      "href": "https://host:9011/services/v2/extracts/EXT1",
      "mediaType": "application/json"
    }
  ]
}

```

## Acknowledgements

* **Author** - TODO: Your Name, Your Title, Your Organization
* **Last Updated By/Date** - TODO: Your Name, Month Year
