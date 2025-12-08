#### TROUBLESHOOT
---
---



##### Collecting failure information
	Log in to the management server as the root user.
	Stop the Common Services service.
	To collect the failure information of Common Services, run the csgetras command.
		/opt/hitachi/CommonService/utility/bin/csgetras.sh -dir path-to-output-destination-directory
		
##### Common Services logs
	/var/log/hitachi/CommonService

##### Changing the properties of logs
	Refer to Hitachi OPS Center Installation and Configuration Guide.pdf

##### Common Services audit log
	The audit log is output to the syslog.

##### Changing the audit log properties
	Refer to Hitachi OPS Center Installation and Configuration Guide.pdf
	- Dump:
		Hitrack
			VPN ->
			https://hitrack.hds.com/
			https://hitrack.hitachivantara.com/
	- Storage Navigator -> Menu -> Maintenance -> Maintenance Components -> Auto Dump
		Upload to http://tuf.hds.com --> click “to upload files”
	- Windows Menu ->SVP -> zsv_AutoDump
		C:\DKC200\mp\PC\zsv_AutoDump.exe
		Dump file is under C:\DKC200\DUMP or under C:\HDS\Dump or under C:\DKC200\tmp\hdcp.tgz
		Upload to http://tuf.hds.com --> click “to upload files”

##### Performance Dump https://support.hitachivantara.com/en/user/answers/downloads.html
	Export tool 
		Download: https://support.hitachivantara.com/en/user/answers/downloads.html
		Storage Type -> Select Firmware -> Customer Tools CD
	Edit command.txt
	Enter svp ip, username, password
	Run runWin.bat or runUnix.bat
	(out folder in the same directory)
