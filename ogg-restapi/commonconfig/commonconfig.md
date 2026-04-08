# 1. Introduction

### Objectives

In this lab, you will:
* TODO: Add objectives


Estimated Time: TODO - x minutes


This document provides a complete reference for all write-operation REST
API endpoints in Oracle GoldenGate 26ai. It covers every POST, PATCH,
and PUT endpoint, documenting each JSON payload field, its type, whether
it is required, a description, and all valid constraint/allowed values.
For every endpoint, multiple worked JSON examples are provided
demonstrating different payload combinations.


**All endpoints follow the base URL pattern:**

> https://\<host\>:\<port\>/services/v2/\<resource\>

Authentication uses HTTP Basic Auth (Administrator or Operator role as
noted per endpoint). All request and response bodies use Content-Type:
application/json.


## 1.1 Method Summary

  |**Method** |  **Purpose**                                                |
  |------------ ------------------------------------------------------------|
  |**POST**   |  Create a new resource or issue a command against an existing one.|
  |**PATCH**  |  Partially update an existing resource (only supplied fields are changed).|
  |**PUT**    | Replace an existing resource in its entirety.|

## Acknowledgements

* **Author** - TODO: Your Name, Your Title, Your Organization
* **Last Updated By/Date** - TODO: Your Name, Month Year
