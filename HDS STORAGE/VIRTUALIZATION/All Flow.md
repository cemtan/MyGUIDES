### VIRTUALIZATION
---
---



#### MIGRATION
---

For disks larger than 4TB, Data Direct Mapping (DDM) pool is required

**1. Change Ports**
	a. Fabric ON
	b. Security ENABLED
	c. Port BIDIRECTIONAL

**2. Define Host Group at Target Storage**
	a. Enable HMO 25 if it is cluster

**3. Define Target Storage as Windows at Source Storage**
	a. Enable HMO 25 if it is cluster

**4. Stop Application**
	a. Make disks offline

**5. Disable Source Storage to Host Zone**
	a. Rescan Disks on Host

**6. Present Disks to Target Ports**
	a. Select Disks and Click "Add LUN Paths"

**7. Run "Discover External Storage" at Target Storage (External Volumes Section)**

**9. Check if Task is Completed**

**10. Create LDEVs**
	**a. For DDM Disks**
		**1. Add External Volume**
			a. Go to External Storage on Device Manager
			b. Clik "Add External Volumes"
			c. Select "By External Path Groups" and select related Path Group ID
			d. Click "Next"
			e. Select "Discovered Volume"
			f. Enable "Data Direct Mapping"
			g. Give LDEV Name
			h. Give LDEV ID
			i. Cache Settings:
				1. Cache Partition: Cache memory can be partitioned by using Virtual Partition Manager to configure a cache logical partition (CLPR) for the mapped volumes. Cache logical partitions are often used to limit cache-use by accessing slower external storage volumes. We strongly recommend that you place external storage array groups in a CLPR other than CLPR0.
				2. Cache Mode: I/O to and from the local storage system always uses cache. Write operations are always backed up in duplex cache. The Cache Mode setting specifies whether write data from the host is written to the external volume asynchronously (Enable) or synchronously (Disable).
					a. Enable: After receiving the data into the local system's cache memory, the system signals the host that the I/O operation has completed, and then asynchronously destages the data to the external volume.
					b. Disable (default): After synchronously writing the data to the external volume, the local system signals the host that an I/O operation has completed.
				3. Inflow Control: When the write operation to the external volume cannot be completed, Inflow Control specifies whether the write operation to cache memory is limited (Enable) or continued (Disable).
					a. Enable: The write operation to cache is limited and I/O from the host is not accepted. Limiting the write operation prevents the accumulation of data that cannot destage to cache memory.
					b. Disable (default): I/O sent from the host during the retry operation is written to cache memory. When write operations to the external volume are again possible, data in cache memory is destaged to the external volume.
			j. Click "Add"
			k. Click "Finish"
		**2. Expand DDM Pool**
			a. Go to DDM Pool → Pool Volumes
			b. Click "Expand Pool"
				1. Drive Type: External Storage
				2. Click "Select Pool VOLs"
				3. Select Volume
				4. Click "Apply"
		**3. Create LDEV**
			a. Go to DDM Pool → Virtual Volumes
			b. Click "Create LDEV"
			c. Select Disk
			d. Click "Add"
			e. Click "Next" (to present LDEV to host)
			f. Give LDEV Name
			g. Give LDEV ID
			h. Click "Add"
			i. Click "Next" (Present to Host)
			j. Select Host Ports
			k. Click "Add"
			l. Click "Next"
			m. Select all "LUN IDs"
			n. Click "Change LUN IDs"
			o. Change "Initial LUN ID"
			p. Click "Apply"
		**4. Add LUN paths to target storage for non-DDM disks**
	**b. Create External Volumes for non-DDM Disks**
		**1. Device Manager**
			a. Go to External Storage on Device Manager
			b. Clik "Add External Volumes"
			c. Select "By External Path Groups" and select related Path Group ID
			d. Click "Next"
			e. Check if LDEV IDs are sorted
			f. Select all LDEVs
			g. Give a Prefix and Initial Number
			h. Give LDEV IDs
			i. Click "Add"
			j. Click "Next" (to present LDEV to host)
			k. Give LDEV Name
			l. Give LDEV ID
			m. Click "Add"
			n. Click "Next" (Present to Host)
			o. Select Host Ports
			p. Click "Add"
			q. Click "Next"
			r. Select all "LUN IDs"
			s. Click "Change LUN IDs"
			t. Change "Initial LUN ID"
			u. Click "Apply"
			v. For non-DDM Disks: Click + Sign to "Create External Volumes"
		**2. OPS Center**
			a. Add all external volumes
			b. Attach to the server

**10. Scan Disks at Host**
	a. Make Disks Online

**11. Run Application**

**12. Migrate Disks**
	a. Go to OPS Center Administrator → External Volumes
	b. Select related Volumes
	c. Click "Actions" → "Migration Volumes"
	d. Enable "Data Reduction Share Settings"
	e. Select Pool
	f. Click "Next"
	g. Capacity Saving → Compression
	h. LDEV ID → Automatic (Temporary ID)
	i. Shred Source Volumes → No
	j. Delete Source Volumes → No
	k. Click "Next"
	l. Migrate Now
	m. Submit

**13. Delete External Volumes if Number of Paths is Zero**

**14. Delete Source Disks**


#### CLEAN UP
---

Provisioning Guide

**1. Go to DDM Pool --> Virtual Volumes**

**2. Find DDM volumes in DDM pool and block them**
	a. LDEV ids are temp ids for migration
	b. Check number of paths (should be 0)
	c. Select LDEVs
	d. More Acitions --> Block LDEVs

**3. Check task**
	a. Environment Settings → Edit information display
	b. Change 60s to 10

**4. Go to external storage**

**5. Find volumes with DDM enabled**
	a. Select LDEVs
	b. More Actions → Disconnect

**6. Check task**

**7. Go to DDM Pool --> Virtual Volumes**
	a. Select DDM LDEVs
	b. More Actions → Delete LDEVs

**8. Check task**

**9. Go to External Volumes**
	a. Select DDM LDEVs
	b. More Actions → Reconnect external volumes

**10. Check task**

**11. Go to DDM Pool --> Pool Volumes**
	a. Select DDM volumes
	b. Shrink Pool

**12. Check task**

**13. Go to Host Group → LUNs**
	a. Check LDEVs
	b. All should be different than external LDEVs

**14. Go to External Volumes**
	a. Select all LDEVs except DDM pool disk (the disk used to create DDM pool in the beginning)
	b. More Actions → Delete External Volumes (no need to disconnect since disks are already migrated)
		1. Change "yes to no" in first option: No need to disconnect
		2. Change "no to yes" in second option: Force to delete

**15. Check task**

**16. Go to Source Storage**
	a. Delete LUN paths of the volumes presented to the destination storage
	b. Do not delete last path until you delete all snapshots

**17. Delete zone of the host to the sorce storage**

**18. Delete migration tasks if you want**



