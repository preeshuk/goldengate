# Service Manager

## Deployments

### **[POST]** /services/v2/deployments/{deployment}

**Required Role:** *Administrator*

**Request Body Parameters**

| Field | Type | Required | Description |
|------|------|----------|------------|
| `$schema` | string | No | `"ogg:deployment"` |
| `oggHome` | string | Yes | OGG installation path |
| `oggVarHome` | string | No | Runtime path |
| `oggConfHome` | string | No | Config directory |
| `oggDataHome` | string | No | Data directory |
| `oggArchiveHome` | string | No | Archive directory |
| `oggSslHome` | string | No | SSL directory |
| `oggEtcHome` | string | No | etc directory |
| `enabled` | boolean | No | Default true |
| `environment[]` | array | No | Env variables |
| `configuration` | object | No | Config service |
| `metrics` | object | No | Metrics config |
| `cluster[]` | array | No | Cluster roles |


**Example**
```json
{
  "$schema": "ogg:deployment",
  "oggHome": "/opt/oracle/goldengate/21.3",
  "enabled": true
}
```

---

### **[PATCH]** /services/v2/deployments/{deployment}

| Field | Type | Description |
|------|------|------------|
| `enabled` | boolean | Enable/disable |
| `oggHome` | string | Update path |
| `environment[]` | array | Update env |
| `metrics` | object | Update metrics |



## Services

### **[POST]** /services/v2/deployments/{deployment}/services/{service}

**Required Role:** *Administrator*

---

**Request Body Parameters**

| Field | Type | Description |
|------|------|------------|
| `$schema` | string | `"ogg:service"` |
| `enabled` | boolean | Default true |
| `critical` | boolean | Default true |
| `status` | string | starting \| running \| stopped |
| `locked` | boolean | Prevent start |
| `quiet` | boolean | Quiet mode |
| `config.workerThreadCount` | integer | Default 24 |
| `config.security` | boolean | Enable HTTPS |
| `config.hstsEnabled` | boolean | Enable HSTS |
| `config.network.serviceListeningPort` | int/object | Port or listener |
| `config.network.ipACL` | array | Access control |
| `restart.enabled` | boolean | Enable restart |
| `restart.delay` | integer | Delay |
| `restart.retries` | integer | Retry count |
| `restart.window` | integer | Time window |
| `restart.disableOnFailure` | boolean | Disable after failure |

---

### Example — Minimal service
```json
{
  "$schema": "ogg:service",
  "enabled": true,
  "config": {
    "network": {
      "serviceListeningPort": 9011
    }
  }
}
```

---

### Example — HTTPS service
```json
{
  "$schema": "ogg:service",
  "enabled": true,
  "critical": true,
  "status": "running",
  "config": {
    "security": true,
    "hstsEnabled": true,
    "workerThreadCount": 32,
    "network": {
      "serviceListeningPort": 9021
    }
  }
}
```

---

### **[PATCH]** /services/v2/deployments/{deployment}/services/{service}

| Field | Type | Description |
|------|------|------------|
| `status` | string | running \| stopped \| restart |
| `enabled` | boolean | Enable/disable |
| `critical` | boolean | Update critical flag |
| `locked` | boolean | Lock service |
| `quiet` | boolean | Quiet mode |
| `config.workerThreadCount` | integer | Update threads |
| `config.security` | boolean | HTTPS toggle |
| `config.network.serviceListeningPort` | int/object | Update port |
| `config.network.ipACL` | array | Replace ACL |
| `restart.enabled` | boolean | Restart toggle |

---

### Example
```json
{ "status": "running" }
```

## 5.3 Authorization Profiles

### **[POST]** /services/v2/deployments/{deployment}/authorization/profiles/{profile}

**Required Role:** *Administrator*

### Request Body Parameters

| Field | Type | Required | Description | Constraints |
|------|------|----------|------------|------------|
| `$schema` | string | No | `"ogg:authorizationProfile"` |
| `issuer` | string | Yes | OIDC issuer URL | HTTPS URL |
| `clientId` | string | Yes | OAuth client ID | string |
| `clientSecret` | string | Yes | Client secret | string |
| `audience` | string | No | JWT audience | string |
| `scope` | string | No | OAuth scopes | space-separated |
| `enabled` | boolean | No | Default true |
| `description` | string | No | Description | max 4095 chars |

---

### Example
```json
{
  "$schema": "ogg:authorizationProfile",
  "issuer": "https://idp.example.com/oauth2",
  "clientId": "ogg-admin-client",
  "clientSecret": "client-secret",
  "enabled": true
}
```

---

## 5.4 Certificates

### **[POST]** /services/v2/deployments/{deployment}/certificates/{type}/{certificate}

| Field | Type | Required | Description |
|------|------|----------|------------|
| `$schema` | string | No | `"ogg:certificate"` |
| `type` | string | Yes | server \| client \| ca |
| `certificate` | string | Yes | PEM cert |
| `privateKey` | string | No | PEM key |
| `passphrase` | string | No | Key passphrase |

---

### Example
```json
{
  "$schema": "ogg:certificate",
  "certificate": "-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----"
}
```

---

## 5.5 User Management

### **[POST]** /services/v2/authorizations/{role}/{user}

| Field | Type | Required | Description |
|------|------|----------|------------|
| `$schema` | string | No | `"ogg:user"` |
| `password` | string | Yes | User password |
| `info` | string | No | Display name |
| `enabled` | boolean | No | Default true |

---

### Example
```json
{
  "$schema": "ogg:user",
  "password": "Admin#Passw0rd",
  "info": "OGG Administrator",
  "enabled": true
}
```

---

## 5.6 AI Management

### **[POST]** /services/v2/installation/aiservice/providers/{provider}

| Field | Type | Required | Description |
|------|------|----------|------------|
| `$schema` | string | No | `"ogg:aiProvider"` |
| `type` | string | Yes | oci \| openai \| azureopenai |
| `endpoint` | string | No | API endpoint |
| `apiKey` | string | No | API key |
| `authentication` | object | No | OCI auth config |
| `enabled` | boolean | No | Default true |
| `description` | string | No | Description |

---

### Example
```json
{
  "$schema": "ogg:aiProvider",
  "type": "openai",
  "endpoint": "https://api.openai.com/v1",
  "apiKey": "sk-xxxx",
  "enabled": true
}
```

---

### **[POST]** /services/v2/installation/aiservice/models/{model}

| Field | Type | Required | Description |
|------|------|----------|------------|
| `$schema` | string | No | `"ogg:aiModel"` |
| `provider` | string | Yes | Provider name |
| `modelId` | string | Yes | Model ID |
| `contextLength` | integer | No | Token limit |
| `enabled` | boolean | No | Default true |
| `description` | string | No | Description |

---

### Example
```json
{
  "$schema": "ogg:aiModel",
  "provider": "openaiProvider",
  "modelId": "gpt-4o",
  "enabled": true
}
```


















## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026
