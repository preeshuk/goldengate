# User Session Endpoints

**\[POST\]** /services/v2/currentuser/reauthorize

**Required Role:** *Authenticated User* \| Reauthorize the current
session (obtain a fresh token without re-login).

**Request Body Parameters**

  | Field    | Type   | Required | Description             | Constraints / Allowed Values |
|----------|--------|----------|-------------------------|------------------------------|
| $schema  | string | No       | Schema discriminator    | "ogg:reauthorize"            |

**Example -- Reauthorize session (no body required)**

```
 {}

```

## Acknowledgements

* **Author** - TODO: Your Name, Your Title, Your Organization
* **Last Updated By/Date** - TODO: Your Name, Month Year
