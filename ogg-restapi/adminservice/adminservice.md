# Administration Service

## Credentials

**\[POST\]** /services/v2/credentials/{domain}/{alias}

**Required Role:** *Administrator* \| Create a new alias in the
credential store.

**Request Body Parameters**

  
  |**Field**    |  **Type** |  **Required** |  **Description**      | **Constraints / Allowed Values**|
  |-------------| ----------| --------------| ----------------------| --------------------------------|
  |\$schema     |  string   | No             | Schema discriminator    | \"ogg:credentials\"|
  |userid       |  string   | Yes            | Database user ID (connection string) |  min 1, max 4096 chars |
  |password     |  string   |  Yes           | Database user password |   min 1, max 1024 chars|

#### Example: Replace Alias Credentials

     
     <copy>
      {
         \"userid\": \"ggadmin@//new-host:1521/NEWDB\",
         \"password\": \"NewP@ss99\"
     }
     
    </copy> 
    
## Database Connections

```
**\[POST\]** /services/v2/connections/{connection}
```

**Required Role:** *Administrator* \

**Description:** Create a new database connection. Connection name follows pattern ***\"domain.alias\"***.


**Request Body Parameters**

  |**Field**             | **Type** |  **Required** |  **Description**       | **Constraints / Allowed Values**     |
  |----------------------| ---------| --------------| ---------------------- |------------------------------------|
  |\$schema              | string   |  No           |  Schema discriminator  | \"ogg:connection\"|
  |credentials.\$schema  | string   | No            | Credential ref discriminator | \"ogg:credentialsRef\"|
  |credentials.domain    | string   |  Yes          |  Credential store domain name     | Default: \"OracleGoldenGate\"; max 30 chars|
  |credentials.alias     | string   |  Yes          |  Credential store alias name | max 30 chars, pattern: \^\[a-zA-Z\]\[a-zA-Z0-9\_#\$\]\*\$|
  
  #### Example: Minimal

  ```
  <copy>
     
  {
    \"credentials\": { 
      \"alias\": \"ggnorth\" }
  }

  </copy>
  ```

#### Example: Full with Explicit Domain and \$schema

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

**Description:** Replace an existing database
connection definition.

**Request Body Parameters**

  |**Field**        |    **Type**  |  **Required**  | **Description** |        **Constraints /Allowed Values**|
  |------------------|-- ---------- -|------------- ---|------------------|-- --------------------------------------|
  |\$schema         |    string    | No             | Schema discriminator  |   \"ogg:connection\" |
  |credentials.domain |   string   |  Yes           | Credential store domain name | Default: \"OracleGoldenGate\"|
  |credentials.alias  |  string    | Yes            | Credential store alias name | max 30 chars |


**Example:  Replace connection**

```
 {

  \"credentials\": {
    \"domain\": \"OracleGoldenGate\",
    \"alias\": \"ggeast\"
   }
 }

```

**\[POST\]** /services/v2/connections/{connection}/tables/heartbeat

**Required Role:** *Administrator* \
**Description:** Create a heartbeat table for the specified database connection.

**Request Body Parameters**

|**Field**   |   **Type**  | **Required** |  **Description**  | **Constraints /Allowed Values**|
  |-------------|- ---------- -|------------- -|------------------- |----------------------------------------------- |
  |schema       |  string      | No            | Database schema to create heartbeat table in | If omitted, default schema is used|
  |table        |  string      | No            | Table name override for heartbeat table |   Defaults to OGG_HEARTBEAT |


**Example 1 -- Default heartbeat table**

> {}

**Example 2 -- Custom schema and table name**

> {
>
> \"schema\": \"GGADMIN\",
>
> \"table\": \"GG_HEARTBEAT_PROD\"
>
> }

**\[POST\]** /services/v2/connections/{connection}/trandata/table

**Required Role:** *Administrator* \| Manage supplemental logging for a
specific table.

**Request Body Parameters**
  |**Field**    |  **Type** |  **Required** |  **Description**  |         **Constraints /|Allowed Values**|
  -------------- ---------- -------------- ------------------------- -------------------------------------
  |schemaTable  |  string   |  Yes          |  Schema.Table to enable supplemental logging on  |  Pattern: SCHEMA.TABLE or SCHEMA.\*|
  | enabled     |   boolean |   Yes         |  Enable (true) or disable  true \| false | (false) supplemental logging |
  |allColumns    | boolean    |No             |Enable supplemental logging on all columns     |Default: false|
  |keyColumns    | array      |No          | List of additional key column names |  Array of strings|

**Example: Enable minimum supplemental logging**

> {
>
> \"schemaTable\": \"HR.EMPLOYEES\",
>
> \"enabled\": true
>
> }

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

> {
>
> \"schemaTable\": \"HR.EMPLOYEES\",
>
> \"enabled\": false
>
> }

## 2.3 Extracts
















## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026
