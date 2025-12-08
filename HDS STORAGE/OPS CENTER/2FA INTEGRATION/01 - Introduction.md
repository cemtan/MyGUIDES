## 2FA INTEGRATION with EXTERNAL KEYCLOAK IDENTITY PROVIDER
---
---
---



### Introduction
---
---

The purpose of this document is to provide an example integration of Hitachi OPS Center with external Non ADFS Identity Provider using SAML and OpenID protocols.
Hitachi OPS Center started to support Non ADFS identity Provider integration with release 11.04 and later


#### Document Scope
---

This document will encompass the following areas
	• Hitachi OPS Center server release 11.04
	• External Keycloak servers
	• Active directory/LDAP


#### Solution Overview
---

- Hitachi OPS Center server version 11.04
- Keycloak server 1 (for SAML) version 26.3.0
- Keycloak server 2 (for OpenID) version 26.3.0
- Active Directory/LDAP/DNS server

|  Role |  Hostname |  Ip Address |  OS |
|---|---|---|---|
|  OPS Center |  merops1104 |  10.10.10.40 |  OL 9.4 |
|  Keycloak 1 (SAML) |  merubuntu22 |  10.10.10.75 |  Ubuntu 22.04 LTS |
|  Keycloak 2 (OpenID) |  merubuntu24 |  10.10.10.76 |  Ubuntu 24.04 LTS |
|           AD/LDAP/DNS           |        merdc2022       |       10.10.10.20       |          Windows 2022         |
