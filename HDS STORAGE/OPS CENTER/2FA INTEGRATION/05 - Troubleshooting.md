## 1. Troubleshooting
---
---
---



1. If you get an  "**Invalid parameter redirect uri**": **REALM** → **Clients** → **Valid redirect URIs**.
	 Keycloak server client settings uri with marked yellow below can not be resolved by keycloak server.
	 
	1. Name can be wrong
	2. Hosts file or DNS entry can be wrong.
	3. OPS Center normally installed access with both Ip address and hostname. But some issues during installation can be caused to access only configured by Ip address.
	4. **Troubleshooting**
		1. Ssh to OPS Center server
		2. Run ```systemctl status csportal```
		3. Check Access URL lines. If only Ip address URL exists then OPS Center portal not responding **hostname** access.
		4. In this case you can change all hostnames in Keycloak client config to ip address like https://merops1104/portal/ change it to https://10.10.10.40/portal or configure OPS Center respond to hostname access also.
			1. To check access URLs ```/opt/hitachi/CommonService/utility/bin/cschgconnect.sh -list```
			2. To enable hostname access change **merops1104** with your OPS Center **hostname**
				   ```/opt/hitachi/CommonService/utility/bin/cschgconnect.sh -h merops1104 -enableip true```
				   ```systemctl restart csportal```

2. If you get "**HTTP 403 forbidden**" in OPS Center mapper config: **REALM** → **Identity providers**.
	 This means that you are selecting the wrong type of mapper which OPS Center cannot configure. For example, If you choose **Advanced attribute** to role than click select role from bottom you will get this error.

	1. **Troubleshooting**
		1. Configure mappings according to this document or OPS Center supported ones
