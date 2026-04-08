# Distribution Service

## Distribution Paths

### **[POST]** /services/v2/sources/{distpath}

**Required Role:** *Administrator*

---

**Request Body Parameters**

| Field | Type | Required | Description |
|------|------|----------|------------|
| `$schema` | string | No | `"ogg:distPath"` |
| `name` | string | No | Path name |
| `description` | string | No | Description |
| `source.name` | string | Yes | Source trail |
| `source.path` | string | Yes | Source path |
| `target.host` | string | Yes | Target host |
| `target.port` | integer | Yes | Target port |
| `target.name` | string | Yes | Target trail |
| `target.protocol` | string | No | ggs \| ws \| wss |
| `begin` | string/object | No | Start position |
| `status` | string | No | running \| stopped |
| `targetInitiated` | boolean | No | Default false |
| `encryptionProfile` | string | No | Profile |
| `options.bufferSize` | integer | No | Buffer size |
| `options.filterDuplicates` | boolean | No | Default false |

---

**Example**
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

**[PATCH]** /services/v2/sources/{distpath}

| Field | Type | Description |
|------|------|------------|
| `status` | string | running \| stopped |
| `description` | string | Update description |
| `options` | object | Update options |

---

## Data Streams

### **[POST]** /services/v2/stream/{streamName}

**Required Role:** *Administrator*

---

**Request Body Parameters**

| Field | Type | Required | Description |
|------|------|----------|------------|
| `$schema` | string | No | `"ogg:dataStream"` |
| `source.name` | string | Yes | Trail name |
| `source.path` | string | Yes | Trail path |
| `format` | string | No | json \| avro \| xml |
| `keyMapping` | string | No | primaryKey \| allColumns |
| `includeMetadata` | boolean | No | Default false |
| `status` | string | No | running \| stopped |
| `description` | string | No | Description |

---

**Example**
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

### **[PATCH]** /services/v2/stream/{streamName}

| Field | Type | Description |
|------|------|------------|
| `status` | string | running \| stopped |
| `description` | string | Update description |
| `format` | string | Change format |

## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026
