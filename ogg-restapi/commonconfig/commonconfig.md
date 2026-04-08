## 6. Configuration

### **[POST]** /services/v2/config/files/{file}

| Field | Type | Required | Description |
|------|------|----------|------------|
| `$schema` | string | No | `"ogg:configFile"` |
| `content` | string | Yes | File content |
| `mimeType` | string | No | MIME type |

---

### Example
```json
{
  "$schema": "ogg:configFile",
  "content": "PARAM1=VALUE1\nPARAM2=VALUE2"
}
```

---

## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026
