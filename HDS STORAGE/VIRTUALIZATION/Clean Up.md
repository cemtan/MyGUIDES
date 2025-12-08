### CLEAN UP
---
---



**1. Go to Host Group → LUNs**
	a. Check LDEVs
	b. All should be different than external LDEVs

**2. Go to DDM Pool --> Virtual Volumes**
	a. Find DDM volumes in DDM pool and block them
		a. LDEV ids are temp ids for migration
		b. Check number of paths (should be 0)
		c. Select LDEVs
		d. More Acitions --> Block LDEVs
	b. Check task
		a. Environment Settings → Edit information display
		b. Change 60s to 10

**3. Go to External Storage → Mapped Volumes**
	a. Find volumes with DDM enabled
	b. Select LDEVs
	c. More Actions → Disconnect
	d. Check task

**4. Go to DDM Pool --> Virtual Volumes**
	a. Select DDM LDEVs
	b. More Actions → Delete LDEVs
	c. Check task

**5. Go to External Storage → Mapped Volumes**
	a. Select DDM LDEVs
	b. More Actions → Reconnect External Volumes
	c. Check task

**6. Go to DDM Pool --> Pool Volumes**
	a. Select DDM volumes
	b. Shrink Pool
	c. Check task

**7. Go to External Storage → Mapped Volumes**
	a. Select all LDEVs except DDM pool disk (the disk used to create DDM pool in the beginning)
	b. More Actions → Delete External Volumes (no need to disconnect since disks are already migrated)
		1. Change "yes to no" in first option: No need to disconnect
		2. Change "no to yes" in second option: Force to delete
	c. Check task

**8. Go to Source Storage**
	a. Delete LUN paths of the volumes presented to the destination storage
	b. Do not delete last path until you delete all snapshots

**9. Delete zone of the host to the sorce storage**

**10. Delete migration tasks if you want**


#### If Virtual Volumes are in Blockade State:
---

 **1. Enable SOM 1083**
 
	raidcom modify system_opt -system_option_mode system -mode_id 1083 -mode enable -Ixxx
	raidcom get system_opt -key mode -lpr system -Ixxx
	
**2. Delete the DP-VOL associated with the blocked External volumes in the DDM Pool.**
	Go to DDM Pool --> Virtual Volumes
		a. Select DDM LDEVs
		b. More Actions → Delete LDEVs
		c. Check task
		
**3. Delete DDM Pool.**
	Go to Pools
		a. Select DDM Pool
		b. More Actions → Delete Pool
		c. Check task
		
**4. Disable SOM 1083**

	raidcom modify system_opt -system_option_mode system -mode_id 1083 -mode disable -Ixxx
	raidcom get system_opt -key mode -lpr system -Ixxx
