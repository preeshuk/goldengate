## Cluster Management

### **[POST]** /services/v2/installation/cluster

| Field | Type | Required | Description |
|------|------|----------|------------|
| `$schema` | string | No | `"ogg:cluster"` |
| `role` | string | Yes | primary \| standby |
| `name` | string | Yes | Cluster name |
| `host` | string | Yes | Host |
| `port` | integer | Yes | Port |

---

### Example
```json
{
  "$schema": "ogg:cluster",
  "name": "prod-cluster",
  "role": "primary",
  "host": "node1.example.com",
  "port": 9001
}
```


## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle Corporation
* **Last Updated By/Date** - : Preeti Shukla, May 2026

