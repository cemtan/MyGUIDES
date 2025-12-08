#### AUDITING
---
---



##### FILE SYSTEM CONFIGURATION
---
File system audit logging is performed and controlled on a per file system basis.
To enable file system auditing on a file system, use the **filesystem-audit** console command or, in the NAS Manager, navigate to **File Services > File Systems Audit Policies**.

	evs-select 1
	filesystem-audit add -s 50MB auditfilesystem



##### EVS CONFIGURATION
---
If audit records are to be collected by a third party service or viewed with Windows Event Viewer, this must be enabled with the **audit-log-consolidated-cache** console command. 

	evs-select 1
	audit-log-consolidated-cache add -s 50MB auditfilesystem

When events are viewed with Windows Event Viewer, NAS file system auditing events are shown in the FS event log.
**Note**: Events for any user who is a member of the Audit Service Accounts local group are excluded from the audit log. Adding the third party auditing sof



##### SMB AUDITING
---
Auditing of SMB is based on the open and close operations. The events logged by HNAS follow the formats described by Microsoft for object access auditing.
For a concise list, see here. For further information, see here.

The following sections describe the supported events and variations from the equivalent Windows events.

###### 560 Object Open
Create / Open Direcory 																																						Create or open a file
![st_hnas00.png](../../.images/st_hnas00.png)![st_hnas01.png](../../.images/st_hnas01.png)![st_hnas05.png](../../.images/st_hnas05.png)![st_hnas06.png](../../.images/st_hnas06.png)
	
###### 562 Handle Closed
Rename a directory																Change security on a directory											Rename a file																		Change security on a file
![st_hnas02.png](../../.images/st_hnas02.png)![st_hnas03.png](../../.images/st_hnas03.png)![st_hnas07.png](../../.images/st_hnas07.png)![st_hnas08.png](../../.images/st_hnas08.png)
	
###### 563 Object Open for Delete
Delete a directory
![st_hnas04.png](../../.images/st_hnas04.png)

###### 564 Object Deleted
Delete a file
![st_hnas09.png](../../.images/st_hnas09.png)



##### CONFIGURE WINDOWS AFTER HNAS SETUP
---

![st_hnas09.png](../../.images/st_hnas10.png)
![st_hnas09.png](../../.images/st_hnas11.png)
