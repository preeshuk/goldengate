# Administration Service

## Credentials

### **\[POST\]** /services/v2/credentials/{domain}/{alias}

**Required Role:** *Administrator* \| Create a new alias in the
credential store.

**Request Body Parameters**

  
  |Field    |  Type |  Required |  Description      | Constraints / Allowed Values|
  |-------------| ----------| --------------| ----------------------| --------------------------------|
  |`$schema`     |  string   | No             | Schema discriminator    | "ogg:credentials"|
  |`userid`       |  string   | Yes            | Database user ID (connection string) |  min 1, max 4096 chars |
  |`password`     |  string   |  Yes           | Database user password |   min 1, max 1024 chars|

---

**Example 1: Minimal (userid + password)**
  
  ```
    {
      "userid": "ggadmin@//db-host:1521/ORCLCDB",
      "password": "Welcome1#"
    }
  
  ```

---

**Example 2: With $schema discriminator** 

  ```
   {
    "$schema": "ogg:credentials",
    "userid": "c##ggadmin@//server1.dc1.example.com:1521/ORCLCDB",
    "password": "P@ssw0rd_DB_A1"
   }

  ```

---

### **\[PUT\]** /services/v2/credentials/{domain}/{alias}

**Required Role:** Administrator   

**Description:** Replace (fully update) an existing alias in the credential store. Same payload as POST.

**Request Body Parameters**

| Field    | Type   | Required | Description                               | Constraints / Allowed Values |
|----------|--------|----------|-------------------------------------------|------------------------------|
| `$schema`  | string | No       | Schema discriminator                      | "ogg:credentials"            |
| `userid`   | string | Yes      | Database user ID (connection string)      | min 1, max 4096 chars        |
| `password` | string | Yes      | Database user password                    | min 1, max 1024 chars        |

---

**Example: Replace Alias Credentials**

     
     <copy>
      {
         \"userid\": \"ggadmin@//new-host:1521/NEWDB\",
         \"password\": \"NewP@ss99\"
     }
     
    </copy> 

---    

## Database Connections

### **\[POST\]** /services/v2/connections/{connection}

**Required Role:** *Administrator* \

**Description:** Create a new database connection. Connection name follows pattern ***\"domain.alias\"***.


**Request Body Parameters**

  |**Field**             | **Type** |  **Required** |  **Description**       | **Constraints / Allowed Values**     |
  |----------------------| ---------| --------------| ---------------------- |------------------------------------|
  |\$schema              | string   |  No           |  Schema discriminator  | \"ogg:connection\"|
  |credentials.\$schema  | string   | No            | Credential ref discriminator | \"ogg:credentialsRef\"|
  |credentials.domain    | string   |  Yes          |  Credential store domain name     | Default: \"OracleGoldenGate\"; max 30 chars|
  |credentials.alias     | string   |  Yes          |  Credential store alias name | max 30 chars, pattern: \^\[a-zA-Z\]\[a-zA-Z0-9\_#\$\]\*\$|
  
  **Example 1: Minimal**

  ```
  <copy>
     
  {
    \"credentials\": { 
      \"alias\": \"ggnorth\" }
  }

  </copy>
  ```

---
 
 ***Example 2: Full with Explicit Domain and \$schema**
 
 ```
  <copy>
   {
     \"\$schema\": \"ogg:connection\",
     \"credentials\": {
     \"\$schema\": \"ogg:credentialsRef\",
     \"domain\": \"OracleGoldenGate\",
     \"alias\": \"ggsouth\"
    }
   }

</copy>

```

**\[PUT\]** /services/v2/connections/{connection}

**Required Role:** *Administrator* \

**Description:** Replace an existing database connection definition.

**Request Body Parameters**

  |**Field**        |    **Type**  |  **Required**  | **Description** |        **Constraints /Allowed Values**|
  |------------------|-- ---------- -|------------- ---|------------------|-- --------------------------------------|
  |`$schema`         |    string    | No             | Schema discriminator  |   "ogg:connection" |
  |`credentials.domain` |   string   |  Yes           | Credential store domain name | Default: "OracleGoldenGate"|
  |`credentials.alias`  |  string    | Yes            | Credential store alias name | max 30 chars |


**Example:  Replace connection**

```
 {

  "credentials": {
    "domain": "OracleGoldenGate",
    "alias": "ggeast"
   }
 }

```
---

**\[POST\]** /services/v2/connections/{connection}/tables/heartbeat

**Required Role:** *Administrator* \

**Description:** Create a heartbeat table for the specified database connection.

**Request Body Parameters**

|**Field**   |   **Type**  | **Required** |  **Description**  | **Constraints /Allowed Values**|
  |-------------|- ---------- -|------------- -|------------------- |----------------------------------------------- |
  |`schema`       |  string      | No            | Database schema to create heartbeat table in | If omitted, default schema is used|
  |`table`        |  string      | No            | Table name override for heartbeat table |   Defaults to OGG_HEARTBEAT |


**Example 1 -- Default heartbeat table**
 
  ```json
    {}
  
  ```
---

**Example 2 -- Custom schema and table name**
 
 ```json
   {
   
     "schema": "GGADMIN",
     "table": "GG_HEARTBEAT_PROD"
   }

  ```
---

### **\[POST\]** /services/v2/connections/{connection}/trandata/table

**Required Role:** *Administrator* 

**Description:** Manage supplemental logging for a specific table.

**Request Body Parameters**

  |**Field**    |  **Type** |  **Required** |  **Description**  |         **Constraints /|Allowed Values**|
  -------------- ---------- -------------- ------------------------- -------------------------------------
  |schemaTable  |  string   |  Yes          |  Schema.Table to enable supplemental logging on  |  Pattern: SCHEMA.TABLE or SCHEMA.\*|
  | enabled     |   boolean |   Yes         |  Enable (true) or disable  true \| false | (false) supplemental logging |
  |allColumns    | boolean    |No             |Enable supplemental logging on all columns     |Default: false|
  |keyColumns    | array      |No          | List of additional key column names |  Array of strings|

**Example: Enable minimum supplemental logging**

```json

  {
 
    "schemaTable": "HR.EMPLOYEES",
    "enabled": true
  }

```

**Example: Enable with all columns**

> {
>
> \"schemaTable\": \"SALES.ORDERS\",
>
> \"enabled\": true,
>
> \"allColumns\": true
>
> }

**Example: Disable supplemental logging**

```
 {
>
> \"schemaTable\": \"HR.EMPLOYEES\",
>
> \"enabled\": false
>
> }

```

## Extracts

### **[POST]** `/services/v2/extracts/{extract}`

**Required Role:** *Administrator*  

**Description:** Create a new Extract process.  

**Extract name:** uppercase, starts with alpha, up to 8 chars.


 **Request Body Parameters**

| Field | Type | Required | Description | Constraints / Allowed Values |
|------|------|----------|------------|------------------------------|
| `$schema` | string | No | Schema discriminator | `"ogg:extract"` |
| `source` | string | No | Source of data | `"tables"` \| `"tranlogs"` |
| `begin` | string | No | Start point for capture | `"now"` \| ISO-8601 timestamp \| `{at:{csn:...}}` |
| `credentials.domain` | string | No | Credential domain | Default: `"OracleGoldenGate"` |
| `credentials.alias` | string | No | Credential alias | max 30 chars |
| `miningCredentials.domain` | string | No | Mining DB credential domain | For downstream mining databases |
| `miningCredentials.alias` | string | No | Mining DB credential alias | max 30 chars |
| `encryptionProfile` | string | No | Encryption profile name | References `ogg:encryptionProfile` config value |
| `registration` | string | No | DB registration type | `"none"` \| `"default"` \| `{containers, csn, share, optimized, replace}` |
| `registration.containers` | array | No | CDB containers to mine (integrated) | Array of container name strings |
| `registration.csn` | integer | No | Start SCN for registration | Long integer or `"CSN.CSN"` string |
| `registration.share` | bool/string | No | Share LogMiner data dictionary | `true` \| `false` \| extract-name string |
| `registration.optimized` | boolean | No | Optimized registration mode | Default: `false` |
| `registration.replace` | boolean | No | Replace existing registration | Default: `false` |
| `passive` | boolean | No | Passive extract mode | Default: `false` |
| `pluginType` | string | No | PostgreSQL replication slot plugin | `"pgoutput"` \| `"test_decoding"` \| `"PGOUTPUT"` \| `"TEST_DECODING"` |
| `replicationSlot` | string | No | PostgreSQL replication slot name | Pattern: `^[A-Za-z_][A-Za-z0-9_]{0,7}_[0-9a-fA-F]{1,16}$` |
| `intent` | string | No | Data capture workflow intent | `"Unidirectional"` \| `"High Availability"` \| `"Disaster Recovery"` \| `"N-Way"` \| `"Sharding"` |
| `critical` | boolean | No | Mark extract as critical | Default: `false` |
| `status` | string | No | Initial process status | `"stopped"` \| `"running"` \| `"starting"` \| `"killed"` \| `"abended"` |
| `rollover` | boolean | No | Rollover to next trail on restart | Only value: `true` |
| `description` | string | No | Human-readable description | max 4095 chars |
| `config` | array | No | Inline parameter file lines | Array of strings; max 32767 items |
| `managedProcessSettings.$schema` | string | No | Managed settings discriminator | `"ogg:managedProcessSettings"` |
| `managedProcessSettings.autoStart.enabled` | boolean | No | Auto-start with Admin Server | Default: `false` |
| `managedProcessSettings.autoStart.delay` | integer | No | Seconds to delay auto-start | `0–3600` |
| `managedProcessSettings.autoRestart.enabled` | boolean | No | Enable auto-restart on failure | Default: `false` |
| `managedProcessSettings.autoRestart.onSuccess` | boolean | No | Restart even on clean exit | Default: `false` |
| `managedProcessSettings.autoRestart.delay` | integer | No | Seconds between restarts | `0–3600` |
| `managedProcessSettings.autoRestart.retries` | integer | No | Max restart attempts per window | `0–3600`; Default: `9` |
| `managedProcessSettings.autoRestart.window` | integer | No | Retry count window (seconds) | `0–604800`; Default: `60` |
| `managedProcessSettings.autoRestart.disableOnFailure` | boolean | No | Disable after all retries exhausted | Default: `true` |
| `targets[].name` | string | No | Two-char trail name | Pattern: `^[A-Za-z][A-Za-z0-9]$` |
| `targets[].path` | string | No | Trail file storage path | max 4096 chars |
| `targets[].sizeMB` | integer | No | Max trail file size in MB | `1–2000`; Default: `2000` |
| `targets[].remote` | boolean | No | Remote trail flag | Default: `false` |
| `targets[].sequence` | integer | No | Starting trail sequence number | `0–999999999` |
| `alias.name` | string | No | Passive extract name on source | Uppercase, max 8 chars |
| `alias.manager.host` | string | No | Manager host (hostname or IP) | max 4095 chars |
| `alias.manager.port` | integer | No | Manager port | `1–65535` |
| `alias.proxy.host` | string | No | Proxy server host | Hostname or IPv4/IPv6 |
| `alias.proxy.port` | integer | No | Proxy server port | `1–65535` |
| `alias.proxy.credentials.alias` | string | No | Proxy credential alias | max 30 chars |

### **[PATCH]** /services/v2/extracts/{extract}

**Required Role:** *Administrator (or Operator for status)*  

**Description:** Partially update an existing Extract. Only supply fields to change.

### Request Body Parameters

| Field | Type | Required | Description | Constraints / Allowed Values |
|------|------|----------|------------|------------------------------|
| `status` | string | No | Change process status | `"running"` \| `"stopped"` \| `"killed"` \| `"abended"` |
| `critical` | boolean | No | Update critical flag | `true` \| `false` |
| `description` | string | No | Update description | max 4095 chars |
| `config` | array | No | Replace parameter lines | Array of strings |
| `managedProcessSettings` | object | No | Update managed settings | Same schema as POST |
| `credentials.alias` | string | No | Change credential alias | max 30 chars |
| `encryptionProfile` | string | No | Change encryption profile | Profile name string or null |

#### Example 1 — Start an Extract
```json
{ "status": "running" }
```

#### Example 2 — Stop an Extract
```json
{ "status": "stopped" }
```

#### Example 3 — Update description and critical flag
```json
{
  "description": "Updated CDC extract for audit",
  "critical": true
}
```

#### Example 4 — Update managed process auto-restart settings
```json
{
  "managedProcessSettings": {
    "autoRestart": {
      "enabled": true,
      "delay": 120,
      "retries": 3,
      "window": 600
    }
  }
}
```

---

## Replicats

**[POST]** /services/v2/replicats/{replicat}

**Required Role:** *Administrator*  

**Description:** Create a new Replicat process.  

**Name:** uppercase, starts with alpha, up to 8 chars.

**Request Body Parameters**

| Field | Type | Required | Description | Constraints / Allowed Values |
|------|------|----------|------------|------------------------------|
| `$schema` | string | No | Schema discriminator | `"ogg:replicat"` |
| `mode.type` | string | No | Replicat mode type | `"nonintegrated"` \| `"integrated"` \| `"coordinated"` |
| `mode.parallel` | bool/string | No | Parallel apply | `true` \| `false` \| `"bidirectional"` \| `"unidirectional"` |
| `mode.maxThreads` | integer | No | Max threads | `1–500`; Default: `25` |
| `mode.threadNumber` | integer | No | Thread number | `1–500` |
| `source.trail` | string | No | Trail path or name | Path string |
| `source.name` | string | No | Trail 2-char name | Pattern: `^[A-Za-z][A-Za-z0-9]$` |
| `begin` | string | No | Start position | `"now"` \| ISO-8601 \| `{sequence, offset}` |
| `begin.sequence` | integer | No | Trail sequence number | `0–999999999` |
| `begin.offset` | integer | No | Offset in trail file | `0–2147483647` |
| `checkpoint.table` | string | No | Checkpoint table | `SCHEMA.TABLE` |
| `credentials.domain` | string | No | Credential domain | Default: `"OracleGoldenGate"` |
| `credentials.alias` | string | No | Credential alias | max 30 chars |
| `encryptionProfile` | string | No | Encryption profile | Profile name or null |
| `registration` | string | No | Registration type | `"none"` \| `"standard"` |
| `intent` | string | No | Workflow intent | `"Unidirectional"` \| `"High Availability"` \| `"Disaster Recovery"` \| `"N-Way"` \| `"Sharding"` |
| `critical` | boolean | No | Mark replicat as critical | Default: `false` |
| `status` | string | No | Initial status | `"stopped"` \| `"running"` \| `"starting"` |
| `synchronized` | boolean | No | Synchronized stop state | `true` \| `false` |
| `description` | string | No | Description | max 4095 chars |
| `config` | array | No | Inline parameter lines | Array of strings |
| `managedProcessSettings` | object | No | Managed settings | Same as Extract |

**Example 1 — Non-integrated Replicat**
```json
{
  "$schema": "ogg:replicat",
  "mode": { "type": "nonintegrated" },
  "credentials": { "alias": "ggtgt" },
  "begin": "now"
}
```

**Example 2 — Integrated Replicat**
```json
{
  "$schema": "ogg:replicat",
  "mode": { "type": "integrated", "parallel": true },
  "credentials": { "alias": "ggtgt" },
  "registration": "standard",
  "begin": "now",
  "description": "Integrated replicat for SALES schema"
}
```
## Encryption Keys

**[POST]** /services/v2/enckeys/{keyName}

**Required Role:** *Administrator*  

**Description:** Create a new encryption key.

**Request Body Parameters**

| Field | Type | Required | Description | Constraints / Allowed Values |
|------|------|----------|------------|------------------------------|
| `$schema` | string | No | Schema discriminator | `"ogg:enckey"` |
| `keyType` | string | Yes | Key algorithm type | `"AES128"` \| `"AES192"` \| `"AES256"` \| `"BLOWFISH"` |
| `key` | string | No | Base64-encoded key value | Auto-generated if omitted |
| `description` | string | No | Key description | max 4095 chars |

### Examples

#### Example 1 — Auto-generated AES256 key
```json
{
  "keyType": "AES256",
  "description": "Production encryption key"
}
```

#### Example 2 — Supplied key value
```json
{
  "$schema": "ogg:enckey",
  "keyType": "AES128",
  "key": "BASE64_ENCODED_KEY_HERE"
}
```

## Trails

### **[POST]** /services/v2/trails/{trail}

**Required Role:** *Administrator*  
Create a new trail definition (2-character name).

### Request Body Parameters

| Field | Type | Required | Description | Constraints |
|------|------|----------|------------|------------|
| `$schema` | string | No | Schema discriminator | `"ogg:trail"` |
| `path` | string | Yes | Filesystem path | max 4096 chars |
| `sizeMB` | integer | No | Max file size | 1–2000 (Default: 2000) |
| `sequenceLength` | integer | No | Filename digits | 6 or 9 (Default: 9) |
| `description` | string | No | Description | max 4095 chars |

---

### Examples

#### Example 1 — Minimal
```json
{
  "path": "./dirdat/ea",
  "sizeMB": 500
}
```

#### Example 2 — Full definition
```json
{
  "$schema": "ogg:trail",
  "path": "/opt/oracle/goldengate/dirdat/rt",
  "sizeMB": 1000,
  "sequenceLength": 9,
  "description": "Main replication trail"
}
```

---

### **[PATCH]** /services/v2/trails/{trail}

| Field | Type | Description | Constraints |
|------|------|------------|------------|
| `sizeMB` | integer | Update max file size | 1–2000 |
| `description` | string | Update description | max 4095 chars |

#### Example
```json
{ "sizeMB": 2000 }
```

---

## Tasks

**[POST]** /services/v2/tasks/{task}

**Required Role:** *Administrator*

**Request Body Parameters**

| Field | Type | Required | Description | Constraints |
|------|------|----------|------------|------------|
| `$schema` | string | No | Schema discriminator | `"ogg:task"` |
| `type` | string | Yes | Task type | `"archive"` \| `"purge"` |
| `enabled` | boolean | No | Enable task | Default: true |
| `schedule` | string | Yes | Cron schedule | e.g. `"0 * * * *"` |
| `target` | object | No | Target config | Depends on type |
| `description` | string | No | Description | max 4095 chars |


**Examples**

#### Archive task
```json
{
  "$schema": "ogg:task",
  "type": "archive",
  "enabled": true,
  "schedule": "0 2 * * *",
  "description": "Nightly archive"
}
```

#### Purge task
```json
{
  "$schema": "ogg:task",
  "type": "purge",
  "enabled": true,
  "schedule": "0 4 * * 0",
  "description": "Weekly purge"
}
```

#### **[PATCH]** /services/v2/tasks/{task}

| Field | Type | Description |
|------|------|------------|
| `enabled` | boolean | Enable/disable |
| `schedule` | string | Update cron |
| `description` | string | Update description |

---

## Commands

### **[POST]** /services/v2/commands/execute

| Field | Type | Required | Description |
|------|------|----------|------------|
| `command` | string | Yes | GGSCI command |

---

### Examples

```json
{ "command": "INFO ALL" }
```

```json
{ "command": "SEND EXTRACT EXT1, STATUS" }
```

---

### Extract Command

**[POST]** /services/v2/extracts/{extract}/command

| Field | Type | Required | Description |
|------|------|----------|------------|
| `command` | string | Yes | START \| STOP \| STATUS \| STATS |

---

### Replicat Command

**[POST]** /services/v2/replicats/{replicat}/command

| Field | Type | Required | Description |
|------|------|----------|------------|
| `command` | string | Yes | START \| STOP \| KILL |




## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026
