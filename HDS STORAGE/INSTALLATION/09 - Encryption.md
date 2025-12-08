### ENCRYPTION
---
---



#### PreConfiguration
---
First, if the pool exists, delete it to change encryption settings

#### License
---
Make sure the license obtained includes the license for encryption.  If the license obtained is a basic license, it does not include encryption. 

![st_enc01.png](../../.images/st_enc01.png)

#### Enable Encryption - SVP
---
---

###### 1. Following information is needed for KMS server
---

	- KMS Ip Address
	- Root Certificate
	- Client Certificate

###### 2. Enable Encryption Environment
---

![st_enc02.png](../../.images/st_enc02.png) ![st_enc03.png](../../.images/st_enc03.png)
![st_enc04.png](../../.images/st_enc04.png)

###### 3. Enable encryption for parity groups and select quick format
---

![st_enc05.png](../../.images/st_enc05.png) ![st_enc06.png](../../.images/st_enc06.png)

###### 4. Download keys if you did not enable KMS server (backup after each disk change)
---

![st_enc07.png](../../.images/st_enc07.png) ![st_enc08.png](../../.images/st_enc08.png)
![st_enc09.png](../../.images/st_enc09.png)

Provide date to backup file name and take backup if disk is replaced.



#### Enable Encryption - API
---
---

##### 1. Enable encryption
---

	curl -v -H "Accept:application/json" -H "Content-Type:application/json" -u maintenance:raid-maintenance -X PATCH -d "{\"isEnabled\": true}" http://172.25.44.XXX/ConfigurationManager/v1/objects/encryption-settings/instance


##### 2. Check the job
---

	curl -k -v -H "Accept:application/json" -H "Content-Type:application/json" -u user-admin:sa4hic -X GET   http://172.25.44.xxx/ConfigurationManager/v1/objects/jobs/12
				* About to connect() to 172.25.44.xxx port 80 (#0)
				* Trying 172.25.44.101...
				* Connected to 172.25.44.101 (172.25.44.101) port 80 (#0)
				* Server auth using Basic with user 'user-admin'
				> GET /ConfigurationManager/v1/objects/jobs/12 HTTP/1.1
				> Authorization: Basic dXNlci1hZG1pbjpzYTRoaWM=
				> User-Agent: curl/7.29.0
				> Host: 172.25.44.101
				> Accept:application/json
				> Content-Type:application/json
				>
				< HTTP/1.1 200 OK
				< Date: Wed, 06 Jan 2021 21:34:15 GMT
				< Content-Type: application/json;charset=utf-8
				< Cache-Control: no-store
				< Pragma: no-cache
				< Content-Length: 552
				< X-Frame-Options: SAMEORIGIN
				< X-XSS-Protection: 1; mode=block
				< X-Content-Type-Options: nosniff
				< Server:
				<
				{
				  "jobId" : 12,
				  "self" : "/ConfigurationManager/v1/objects/jobs/12",
				  "userId" : "user-admin",
				  "status" : "Completed",
				  "state" : "Succeeded",
				  "createdTime" : "2021-01-06T21:27:48Z",
				  "updatedTime" : "2021-01-06T21:33:33Z",
				  "completedTime" : "2021-01-06T21:33:33Z",
				  "request" : {
					"requestUrl" : "/ConfigurationManager/v1/objects/encryption-settings/instance",
					"requestMethod" : "PATCH",
					"requestBody" : "{\"isEnabled\": true}"
				  },
				  "affectedResources" : [ "/ConfigurationManager/v1/objects/encryption-settings/instance" ]
				* Connection #0 to host 172.25.44.xxx left intact


##### 3. Backup encryption keys
---
	
	date=$(date +%Y%m%d)
	
	curl -k -v -H "Accept:application/octet-stream" -H "Content-Type:application/json" -u maintenance:raid-maintenance -d "{\""parameters\":{\"password\":\"backuppassword\""}}" -X PUT http://172.13.15.20/ConfigurationManager/v1/objects/storages/900000061726/encryption-keys/file/actions/backup/invoke -o "/EncKeys/backupfile_61726_${date}.ekf"
	
	find /EncKeys/*.ekf -type f -mtime "+$(( ( $(date '+%s') - $(date -d '3 months ago' '+%s') ) / 86400 ))" -delete


   **1. Create InputParameters.json file**
   ```json
{
	"parameters": {
	"password": "backuppassword"
	}
}
```
   where backuppassword:
   			Specify a character string consisting of 6 to 255 characters.
			You can use the following characters:
			* Alphanumeric characters
			* ! " # $ % & ' ( ) * + , - . / : ; < = > ? @ [ \ ] ^ _ ` { | } ~
	You will need this password when you restore the encryption key.

   **2. Take a backup:**
	
	curl -k -v -H "Accept:application/octet-stream" -H "Content-Type:application/json" -u maintenance:raid-maintenance -X PUT --data-binary @./InputParameters.json http://<CONTROLLER-IP>/ConfigurationManager/v1/objects/storages/<STORAGE-DEVICE-ID>/encryption-keys/file/actions/backup/invoke -o "backupfile.ekf"

   where
			- **InputParameters.json**: This is the file you prepared. Run the curl command inside the directory where you put this file.
			- **<CONTROLLER-IP>**: Ip of controller. Try to access controller by http or https protocol.
			- **<STORAGE-DEVICE-ID>**: This is 12 digit number: The last 6 digit is storage serial number. It is the 0 and storage serial number  for VSP5000 series. The first 6 digits are described in following table as per  https://knowledge.hitachivantara.com/Documents/Storage/VSP_5000_Series/90-08-2x/REST_API_Reference_Guide/01_Overview_of_the_REST_API 
			- **backupfile.ekf**: Backup file needs to be secured with backup password.

|  Storage System |  Fixed Value |
|---|---|
|  VSP 5100, 5500, 5100H, 5500H, VSP 5200, 5600, 5200H, 5600H |  900000[^1] |
|  VSP E590, VSP E790, VSP E590H, VSP E790H |  934000 |
|  VSP E990 |  936000 |
|  VSP E1090, VSP E1090H |  938000 |
|  VSP F370, F700, F900, VSP G370, G700, G900 |  886000 |
|                       VSP F350, VSP G350                      |     882000    |
|                       VSP G130[^2]                      |     880000    |
		

[^1]: These storage systems have serial numbers that are less than 6 digits. Pad the storage device ID with zeros, added after this designated value and before the serial number, to bring the total number of digits for the storage device ID up to 12.

[^2]: This model is available only in select markets. Contact your local sales representative for availability.