# 1. Introduction

### Objectives

In this lab, you will be provided a complete reference for all write-operation REST API endpoints in Oracle GoldenGate 26ai. It covers every **POST**, **PATCH**, and **PUT** endpoint, documenting each JSON payload field, its type, whether it is required, a description, and all valid constraint/allowed values.

For every endpoint, multiple worked JSON examples are provided
demonstrating different payload combinations.:

|NOTE: All endpoints follow the base URL pattern:|
----------------------------------------------------------------
https://\<host\>:\<port\>/services/v2/\<resource\>

Authentication uses HTTP Basic Auth (Administrator or Operator role as
noted per endpoint). All request and response bodies use ***Content-Type:application/json***.


### 1.1 Method Summary

|**Method** | **Purpose**|
|-----------|------------------------------------------------------------------------------|
| POST | Create a new resource or issue a command against an existing one.|
| PATCH | Partially update an existing resource (only supplied fields are changed).|
| PUT    | Replace an existing resource in its entirety.|


## Acknowledgements

* **Author** - : Preeti Shukla, Consulting Technical Writer, Oracle GoldenGate, Werner He, Senior Director
* **Last Updated By/Date** - : Preeti Shukla, May 2026
