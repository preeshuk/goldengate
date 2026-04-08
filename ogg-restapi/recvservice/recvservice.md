# Receiver Service

## Receiver Paths

### **[POST]** /services/v2/targets/{path}

**Required Role:** *Administrator*  

**Description:** Create a new Receiver path.

**Request Body Parameters**

| Field | Type | Required | Description | Constraints |
|------|------|----------|------------|------------|
| `$schema` | string | No | Schema discriminator | `"ogg:recvPath"` |
| `source.host` | string | Yes | Source host | hostname \| IPv4 \| IPv6 |
| `source.port` | integer | Yes | Source port | 1–65535 |
| `source.name` | string | No | Source trail name | 2-char |
| `target.name` | string | Yes | Local trail name | Pattern |
| `target.path` | string | Yes | Local path | max 4096 chars |
| `target.sizeMB` | integer | No | Max size | 1–2000 |
| `status` | string | No | Initial status | running \| stopped |
| `encryptionProfile` | string | No | Encryption profile | string |
| `description` | string | No | Description | max 4095 chars |

---

**Example 1: Minimal Receiver Path**

```json
{
  "source": {
    "host": "distrib-server.example.com",
    "port": 9021
  },
  "target": {
    "name": "ra",
    "path": "./dirdat/ra"
  }
}
```

---

**Example 2: Receiver path with encryption and auto-start**

```
{
  "$schema": "ogg:recvPath",
  "source": {
    "host": "distrib.dc1.example.com",
    "port": 9021,
    "name": "ea"
  },
  "target": {
    "name": "ra",
    "path": "/opt/oracle/gg/dirdat/ra",
    "sizeMB": 1000
  },
  "encryptionProfile": "aes256profile",
  "status": "running",
  "description": "Encrypted receiver path from DC1"
}

```

---

### **[PATCH]** /services/v2/targets/{path}

**Required Role:** Administrator (or Operator for status)   

**Description:** Update an existing Receiver path.

**Request Body Parameters**


| Field | Type | Description |
|------|------|------------|
| `status` | string | running \| stopped |
| `description` | string | Update description |

---

***Example 1: Start a receiver path**
```
{ "status": "running" }

```
---

**Example 2: Stop a receiver path**
{ "status": "stopped" }

## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026
