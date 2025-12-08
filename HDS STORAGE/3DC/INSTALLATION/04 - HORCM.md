### HORCM
---
---



#### SETUP
---

Due to CCI Installation and Configuration Guide:
	  Command device size should be greater than 36MG (Use 1G at least)
	  Command device name should be raw device (character-type device) like /dev/rdsk/c0t0d1s2 in Solaris
	  Command device name should be raw device CMD-Ser#-ldev#-Port# in Windows (CMD-511111-52735-CL1-A-1)
	  Command device name should be block device like /dev/sd.. in Linux



#### HORCM FILES
---

##### BEFORE GAD SETUP
---

###### /etc/horcm0.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31000	1000			3000
	
	HORCM_CMD
	# dev_name
	#\\.\IPCMD-172.30.255.28-31001 \\.\IPCMD-172.30.255.29-31001
	/dev/sdc

###### /etc/horcm1.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31001	1000			3000
	
	HORCM_CMD
	# dev_name
	#\\.\IPCMD-172.30.255.38-31001 \\.\IPCMD-172.30.255.39-31001
	/dev/sdd

###### /etc/horcm100.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31100	1000			3000
	
	HORCM_CMD
	# dev_name
	# \\.\IPCMD-172.30.255.28-31001  \\.\IPCMD-172.30.255.29-31001
	/dev/sdc

	HORCM_VCMD
	715996

###### /etc/horcm101.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31101	1000			3000
	
	HORCM_CMD
	# dev_name
	#\\.\IPCMD-172.30.255.38-31001 \\.\IPCMD-172.30.255.39-31001
	/dev/sdd
	
	HORCM_VCMD
	715996

###### /etc/horcm2.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31102	1000			3000
	
	HORCM_CMD
	# dev_name
	\\.\IPCMD-172.30.35.11-31001 \\.\IPCMD-172.30.35.12-31001


##### AFTER GAD SETUP
---

###### /etc/horcm0.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31000	1000			3000
	
	HORCM_CMD
	# dev_name
	#\\.\IPCMD-172.30.255.28-31001 \\.\IPCMD-172.30.255.29-31001
	/dev/sdc
	
	HORCM_LDEV
	#GRP				DEV					SERIAL	LDEV#	MU#
	bvmint02			BVMINT02_POOL01		745175	00:00		h0
	bvmint02			BVMINT02_POOL02		745175	00:01		h0
	
	HORCM_INST
	#GPR				IP ADR		PORT#		pathID 
	bvmint02			localhost	31001

###### /etc/horcm1.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31001	1000			3000
	
	HORCM_CMD
	# dev_name
	#\\.\IPCMD-172.30.255.38-31001 \\.\IPCMD-172.30.255.39-31001
	/dev/sdd
	
	HORCM_LDEV
	#GRP				DEV					SERIAL	LDEV#	MU#
	bvmint02			BVMINT02_POOL01		715062	00:00		h0
	bvmint02			BVMINT02_POOL02		715062	00:01		h0
	
	HORCM_INST
	#GPR				IP ADR		PORT#		pathID 
	bvmint02			localhost	31000

###### /etc/horcm2.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31102	1000			3000
	
	HORCM_CMD
	# dev_name
	\\.\IPCMD-172.30.35.11-31001 \\.\IPCMD-172.30.35.12-31001


##### AFTER 3DC SETUP
---

###### /etc/horcm0.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31000	1000			3000
	
	HORCM_CMD
	# dev_name
	#\\.\IPCMD-172.30.255.28-31001 \\.\IPCMD-172.30.255.29-31001
	/dev/sdc
	
	HORCM_LDEV
	#GRP				DEV					SERIAL	LDEV#	MU#
	bvmint02			BVMINT02_POOL01		745175	00:00		h0
	bvmint02			BVMINT02_POOL02		745175	00:01		h0
	bvmint02_HUR			BVMINT02_POOL01_HUR	745175	00:00		h2
	bvmint02_HUR			BVMINT02_POOL02_HUR	745175	00:01		h2
	
	HORCM_INST
	#GPR				IP ADR		PORT#		pathID 
	bvmint02			localhost	31001
	bvmint02_HUR			localhost	31002

###### /etc/horcm1.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31001	1000			3000
	
	HORCM_CMD
	# dev_name
	#\\.\IPCMD-172.30.255.38-31001 \\.\IPCMD-172.30.255.39-31001
	/dev/sdd
	
	HORCM_LDEV
	#GRP				DEV					SERIAL	LDEV#	MU#
	bvmint02			BVMINT02_POOL01		715062	00:00		h0
	bvmint02			BVMINT02_POOL02		715062	00:01		h0
	bvmint02_DLT			BVMINT02_POOL01_DLT	715062	00:00		h3
	bvmint02_DLT			BVMINT02_POOL02_DLT	715062	00:01		h3
	
	HORCM_INST
	#GPR				IP ADR		PORT#		pathID 
	bvmint02			localhost	31000
	bvmint02_DLT			localhost	31002

###### /etc/horcm2.conf 
	HORCM_MON
	#ip_address	service	poll(10ms)		timeout(10ms)
	localhost		31002	1000			3000
	
	HORCM_CMD
	# dev_name
	\\.\IPCMD-172.30.35.11-31001 \\.\IPCMD-172.30.35.12-31001
	
	HORCM_LDEV
	#GRP				DEV					SERIAL	LDEV#	MU#
	bvmint02_HUR		BVMINT02_POOL01_HUR	715061	00:00		h2
	bvmint02_HUR		BVMINT02_POOL02_HUR	715061	00:01		h2
	bvmint02_DLT		BVMINT02_POOL01_DLT	715061	00:00		h3
	bvmint02_DLT		BVMINT02_POOL02_DLT	715061	00:01		h3
	
	
	HORCM_INST
	#GPR			IP ADR		PORT#		pathID 
	bvmint02_HUR		localhost	31000
	bvmint02_DLT		localhost	31001

	

#### STARTUP
---

##### Start horm instances and login
	horcmstart 0 1 2
	raidcom -login horcm Passw00rd -IH0
	raidcom -login horcm Passw00rd -IH1
	raidcom -login horcm Passw00rd -IH2

##### Get licenses
	raidcom get license -IH0
	raidcom get license -IH1
	raidcom get license -IH2

##### Get port information for zoning
	raidcom get port -IH0
	raidcom get port -IH1
	raidcom get port -IH2
	