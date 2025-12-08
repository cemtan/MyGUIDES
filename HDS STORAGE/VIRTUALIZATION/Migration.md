### MIGRATION
---
---



For disks larger than 4TB, Data Direct Mapping (DDM) pool is required

**1. Define Ports**
	a. Fabric ON
	b. Security ENABLED
	c. Port BIDIRECTIONAL (on destination)

**2. Define Host Group at Target Storage** 
	a. Enable HMO 25 if it is cluster

**3. Define Target Storage as Windows at Source Storage**
	a. WIN_EX, 2,7,22,25,40

**4. Stop Application**
	a. Make disks offline
	b. Check disk reservations on clusters (when HMO 25 is not set)

**5. Disable Source Storage to Host Zone**
	a. Rescan Disks on Host

**6. Present Disks to Target Ports**
	a. Select Disks and Click "Add LUN Paths"

**7. Run "Discover External Storage" at Target Storage (External Volumes Section)**

**8. Check if Task is Completed**

**9. Add External Volumes**
	**a. For DDM Disks**
		**1. Add External Volume**
			a. Go to External Storage on Device Manager
			b. Clik "Add External Volumes"
			c. Select "By External Path Groups" and select related Path Group ID
			d. Click "Next"
			e. Select "Discovered Volume"
			f. Enable "Data Direct Mapping"
			g. Give LDEV Name
			h. Give LDEV ID (Temp Pool Volume ID for DDM Disks)
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
				4. Click "Add"
				5. Click "OK"
				6. Click "Apply"
		**3. Create LDEV**
			a. Go to DDM Pool → Virtual Volumes
			b. Click "Create LDEV"
			c. Select Disk
			d. Give LDEV Name
			e. Give LDEV ID (FINAL)
			f. Click "Add"
			g. Click "Next" (Present to Host)
			h. Select Host Ports
			i. Click "Add"
			j. Click "Next"
			k. Select all "LUN IDs"
			l. Click "Change LUN IDs"
			m. Change "Initial LUN ID"
			n. Click "Apply"
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
			h. Give LDEV ID (FINAL)
			i. Click "Add"
			j. Click "Next" (Present to Host)
			k. Select Host Ports
			l. Click "Add"
			m. Click "Next"
			n. Select all "LUN IDs"
			o. Click "Change LUN IDs"
			p. Change "Initial LUN ID"
			q. Click "Apply"
			r. For non-DDM Disks: Click + Sign to "Create External Volumes"
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
