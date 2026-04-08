# Distribution Service

## Distribution Paths

### **[POST]** /services/v2/sources/{distpath}

**Required Role:** *Administrator*

---

**Request Body Parameters**

| Field | Type | Required | Description | Constraints / Allowed Values|
|------|------|----------|------------|-------------------------------|
| `$schema` | string | No | `"ogg:distPath"` | "ogg:distPath" |
| `name` | string | No | Path name | max 4095 chars |Pattern: ^[A-Za-z][A-Za-z0-9-_.]*$\
| `description` | string | No | Description | Path description| max 4095 chars |
| `source.name` | string | Yes | Source trail | Pattern: ^[A-Za-z][A-Za-z0-9]$ |
| `source.path` | string | Yes | Source path | hostname | IPv4 | IPv6 |
| `target.host` | string | Yes | Target host | 1–65535 |
| `target.port` | integer | Yes | Target port | Pattern: ^[A-Za-z][A-Za-z0-9]$ |
| `target.name` | string | Yes | Target trail | Path string |
| `target.protocol` | string | No | ggs \| ws \| wss |"ggs" | "wss" | "ws" | "ogg"|
| `begin` | string/object | No | Start position | "now" | ISO-8601 | {sequence, offset} |
| `status` | string | No | running \| stopped | "running" | "stopped" | "uninitialized" |
| `targetInitiated` | boolean | No | Default false | Default: false |
| `encryptionProfile` | string | No | Profile | References ogg:encryptionProfile |
| `options.bufferSize` | integer | No | Buffer size | Positive integer |
| `options.filterDuplicates` | boolean | No | Default false | Default: false |
| `ruleset` | object | No | Table/schema routing ruleset | Ruleset definition object |

---

**Example 1: Minimal GGS distribution path**

```json
{
  "$schema": "ogg:distPath",
  "source": { "name": "ea", "path": "./dirdat/ea" },
  "target": {
    "host": "target.example.com",
    "port": 9021,
    "name": "ra"
  },
  "begin": "now"
}
```

---

**Example 2: WSS (TLS) distribution path with begin at sequence**

```
   {
  "$schema": "ogg:distPath",
  "source": { "name": "ea", "path": "./dirdat/ea" },
  "target": {
    "host": "tgt.example.com",
    "port": 443,
    "name": "ra",
    "protocol": "wss"
  },
  "begin": {
    "$schema": "type:position/atTrailRBA",
    "sequence": 50,
    "offset": 0
  },
  "encryptionProfile": "aes256profile",
  "description": "Secure TLS distribution path"
  }

```
---

**Example 3: Target-initiated path**

```
  {
  "$schema": "ogg:distPath",
  "source": { "name": "ea", "path": "./dirdat/ea" },
  "target": {
    "host": "receiver.example.com",
    "port": 9021,
    "name": "ra"
  },
  "targetInitiated": true,
  "status": "running"
}

```


### **[PATCH]** /services/v2/sources/{distpath}

**Required Role:** Administrator (or Operator for status)   

**Description:** Update an existing Distribution Path.

**Request Body Parameters**


| Field | Type | Description | Constraints / Allowed Values|
|------|------|------------|-------------------------------|
| `status` | string | running \| stopped | "running" | "stopped" | "killed"|
| `description` | string | Update description | max 4095 chars |
| `options` | object | Update options | bufferSize, filterDuplicates |
| `target.port` | integer | No | Change target port | 1–65535 |

---

**Example 1: Start a distribution path**

```
{ "status": "running" }

```
---

**Example 2: Stop a distribution path** 

```
{ "status": "stopped" }

```

---

**Example 3: Update CSN position and start**

```
{
  "status": {
    "desired": "running",
    "at": 7000001
  }
}
```
---

## Data Streams

### **[POST]** /services/v2/stream/{streamName}

**Required Role:** *Administrator*

**Description:** Create a new GoldenGate Data Stream configuration (Kafka-style streaming). 

**Request Body Parameters**


| Field            | Type    | Required | Description                     | Constrains / Allowed Values      |
|------------------|---------|----------|---------------------------------|----------------------------------|
| `$schema`        | string  | No       | Schema discriminator            | `"ogg:dataStream"`               |
| source.name      | string  | Yes      | Source trail name               | 2-character trail name           |
| source.path      | string  | Yes      | Source trail path               | Max 4096 characters              |
| format           | string  | No       | Output message format           | `json` \| `avro` \| `xml`        |
| keyMapping       | string  | No       | Message key mapping strategy    | `primaryKey` \| `allColumns`     |
| includeMetadata  | boolean | No       | Include OGG metadata in message | Default: false                   |
| status           | string  | No       | Initial stream status           | `running` \| `stopped`           |
| description      | string  | No       | Stream description              | Max 4095 characters              |

---

**Example 1: JSON Data Stream**

```json
{
  "$schema": "ogg:dataStream",
  "source": { "name": "ea", "path": "./dirdat/ea" },
  "format": "json",
  "keyMapping": "primaryKey",
  "includeMetadata": true,
  "status": "running"
}
```
---

**Example 2: Avro format stream**

```
 {
  "source": {
    "name": "ra",
    "path": "./dirdat/ra"
  },
  "format": "avro",
  "description": "Avro stream for Kafka integration"
 }

```

### **[PATCH]** /services/v2/stream/{streamName}

**Required Role:** *Administrator* \

**Description:** Update an existing Data Stream configuration.

**Request Body Parameters**

| Field       | Type   | Required | Description             | Constrains / Allowed Values      |
|------------|--------|----------|-------------------------|----------------------------------|
| status      | string | No       | Change stream status    | `running` \| `stopped`           |
| description | string | No       | Update description      | Max 4095 characters              |
| format      | string | No       | Change output format    | `json` \| `avro` \| `xml`        |

**Example: Start a data stream**

```
 { \"status\": \"running\" }

```

**\[PATCH\]** /services/v2/stream/{streamName}/yaml

**Required Role:** *Administrator* \

**Description:** Update the AsyncAPI YAML specification for a Data Stream.

**Request Body Parameters**

  ### Request Body Parameters

| Field | Type   | Required | Description                     | Constrains / Allowed Values |
|-------|--------|----------|---------------------------------|-----------------------------|
| yaml  | string | Yes      | AsyncAPI YAML specification content | Full YAML document string   |

**Example: Update AsyncAPI spec**

```
 {

 \"yaml\": \"asyncapi: 2.6.0\\ninfo:\\n title: OGG Stream\\n version:
 1.0.0\"

 }

```
---

## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026
