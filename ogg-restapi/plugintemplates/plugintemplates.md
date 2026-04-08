# Plugin Templates

### \[POST\]**/services/v2/deployments/{deployment}/plugin/templates/{plugin}

**Required Role:** *Administrator* \

**Description:** Create a plugin template for an installed plugin within a deployment.

**Request Body Parameters**

| Field       | Type    | Required | Description                 | Constrains / Allowed Values                  |
|------------|---------|----------|-----------------------------|---------------------------------------------|
| `$schema`  | string  | No       | Schema discriminator        | `"ogg:pluginTemplate"`                      |
| config     | object  | Yes      | Plugin configuration        | Plugin-specific configuration object (schema varies per plugin) |
| enabled    | boolean | No       | Enable plugin template      | Default: true                               |
| description| string  | No       | Template description        | Max 4095 characters                         |

**Example 1: Create plugin template**

```
  {
  "$schema": "ogg:pluginTemplate",
  "config": {
    "pluginSpecificParam": "value"
  },
  "enabled": true,
  "description": "Custom plugin template"
}

```

**\[PUT\]**
/services/v2/deployments/{deployment}/plugin/templates/{plugin}

**Required Role:** *Administrator* \| Replace an existing plugin
template.

**Request Body Parameters**

  ------------------------------------------------------------------------------------
  **Field**      **Type**   **Required**   **Description**           **Constraints /
                                                                     Allowed Values**
  -------------- ---------- -------------- ------------------------- -----------------
  config         object     Yes            New plugin configuration  Plugin-specific
                                                                     configuration
                                                                     object

  enabled        boolean    No             Enable plugin template    true \| false

  description    string     No             Template description      max 4095 chars
  ------------------------------------------------------------------------------------

**Example -- Replace plugin template**

> {
>
> \"config\": { \"updatedParam\": \"updatedValue\" },
>
> \"enabled\": true
>
> }

## Acknowledgements

* **Author** - TODO: Your Name, Your Title, Your Organization
* **Last Updated By/Date** - TODO: Your Name, Month Year
