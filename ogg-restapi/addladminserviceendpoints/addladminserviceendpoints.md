# Additional Administration Service Endpoints

## Instantiation CSN

**[POST]**  
`/services/v2/connections/{connection}/databases/{database}/{schema}/{table}/instantiationCsn`

**Required Role:** *Administrator*  
**Description:** Set the instantiation SCN/CSN for a specific table to coordinate initial load with change capture.

**Request Body Parameters**

| Field  | Type           | Required | Description                     | Constraints / Allowed Values            |
|--------|----------------|----------|---------------------------------|----------------------------------------|
| csn    | integer/string | Yes      | System Change Number (SCN)      | Integer CSN or `"CSN.SUBSCN"` format   |
| status | string         | No       | Instantiation status            | `"pending"` \| `"completed"`            |

### Example 1: Set instantiation CSN (integer)

```json
{
  "csn": 7245891
}

```

### Example 2: Set instantiation CSN with alternate format

```
{
  "csn": "7245891.1024",
  "status": "completed"
}

```

## Schema and Procedure Supplemental Logging

**Schema-Level Logging**

### [POST]/services/v2/connections/{connection}/trandata/schema

**Required Role:** Administrator
**Description:** Manage supplemental logging at the schema level.

**Request Body Parameters**

| Field      | Type    | Required | Description              | Constraints / Allowed Values |
| ---------- | ------- | -------- | ------------------------ | ---------------------------- |
| schema     | string  | Yes      | Schema name to configure | Database schema name         |
| enabled    | boolean | Yes      | Enable/disable logging   | true | false                 |
| allColumns | boolean | No       | Log all columns          | Default: false               |

---

### Example: Enable schema-level supplemental logging

```
{
  "schema": "HR",
  "enabled": true,
  "allColumns": false
}

```

**Procedure-Level Logging**

### [POST]/services/v2/connections/{connection}/trandata/procedure

**Required Role:** Administrator
**Description:** Manage procedural supplemental logging (for stored procedures).

**Request Body Parameters**

| Field     | Type    | Required | Description                       | Constraints / Allowed Values |
| --------- | ------- | -------- | --------------------------------- | ---------------------------- |
| procedure | string  | Yes      | Procedure name (SCHEMA.PROCEDURE) | Fully qualified name         |
| enabled   | boolean | Yes      | Enable/disable procedural logging | true | false                 |


### Example: Enable procedural supplemental logging

```
{
  "procedure": "HR.UPDATE_EMPLOYEE",
  "enabled": true
}

```

## Checkpoint Tables

### [POST]/services/v2/connections/{connection}/tables/checkpoint

**Required Role:** Administrator

**Description:** Manage checkpoint tables for Replicat state storage.

**Request Body Parameters**

| Field  | Type   | Required | Description                 | Constraints / Allowed Values |
| ------ | ------ | -------- | --------------------------- | ---------------------------- |
| action | string | Yes      | Operation to perform        | `"add"` | `"delete"`         |
| schema | string | No       | Schema for checkpoint table | Default schema used          |
| table  | string | No       | Checkpoint table name       | Default: `GGS_CHECKPOINT`    |


### Example 1: Add checkpoint table

```
{
  "action": "add",
  "schema": "GGADMIN",
  "table": "GGS_CHECKPOINT"
}

```

### Example 2 — Delete checkpoint table

```
{
  "action": "delete"
}

```

## Update Heartbeat Table

### [PATCH]/services/v2/connections/{connection}/tables/heartbeat

**Required Role:** Administrator
**Description:** Update heartbeat table configuration.

**Request Body Parameters**

| Field         | Type    | Required | Description                  | Constraints / Allowed Values |
| ------------- | ------- | -------- | ---------------------------- | ---------------------------- |
| enabled       | boolean | No       | Enable/disable heartbeat     | true | false                 |
| frequency     | integer | No       | Heartbeat interval (seconds) | 1–3600                       |
| retentionDays | integer | No       | Record retention (days)      | 1–365                        |


### Example: Update heartbeat frequency

```
{
  "enabled": true,
  "frequency": 60,
  "retentionDays": 7
}

```

## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026
