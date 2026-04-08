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

**Required Role:** *Administrator* \| Replace an existing database
connection definition.

**Request Body Parameters**




## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026
