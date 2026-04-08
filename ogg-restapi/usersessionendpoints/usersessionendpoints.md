# User Session Endpoints

**\[POST\]** /services/v2/currentuser/reauthorize

**Required Role:** *Authenticated User* \

**Description:** Reauthorize the current session (obtain a fresh token without re-login).

**Request Body Parameters**

  | Field    | Type   | Required | Description             | Constraints / Allowed Values |
|----------|--------|----------|-------------------------|------------------------------|
| $schema  | string | No       | Schema discriminator    | "ogg:reauthorize"            |

**Example: Reauthorize session (no body required)**

```
 {}

```

## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026