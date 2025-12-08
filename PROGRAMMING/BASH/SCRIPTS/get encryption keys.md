#### GET ENCRYPTION KEYS
---
---


```bash
#!/bin/bash

: '
This script is cretaed
	- to take a backup of VSP5600 Encryption Keys
	- to delete backup files older than 3 months
IP address is SVP
'

date=$(date +%Y%m%d)

token=$(curl -k -v -H "Accept:application/json" -H "Content-Type:application/json" -u opsconfmgr:VpYkujsWN8HGZf -X POST https://10.13.15.162/ConfigurationManager/v1/objects/sessions/ -d "" | grep token | cut -d '"' -f 4)
curl -k -v -H "Accept:application/octet-stream" -H "Content-Type:application/json" -H "Authorization:Session ${token}" -d "{\""parameters\":{\"password\":\"backuppassword\""}}" -X PUT https://10.13.15.162/ConfigurationManager/v1/objects/encryption-keys/file/actions/backup/invoke -o "/EncKeys/backupfile_61726_${date}.ekf"

token=$(curl -k -v -H "Accept:application/json" -H "Content-Type:application/json" -u opsconfmgr:VpYkujsWN8HGZf -X POST https://10.13.15.163/ConfigurationManager/v1/objects/sessions/ -d "" | grep token | cut -d '"' -f 4)
curl -k -v -H "Accept:application/octet-stream" -H "Content-Type:application/json" -H "Authorization:Session ${token}" -d "{\""parameters\":{\"password\":\"backuppassword\""}}" -X PUT https://10.13.15.163/ConfigurationManager/v1/objects/encryption-keys/file/actions/backup/invoke -o "/EncKeys/backupfile_61735_${date}.ekf"

find /EncKeys/*.ekf -type f -mtime "+$(( ( $(date '+%s') - $(date -d '6 months ago' '+%s') ) / 86400 ))" -delete
```