### Use Case 2. Observe automatic space reclaim after In-guest OS data deletion
---
---



Sample Test Setup Details:

#### 1. Deploy a VM RHEL 7.2 VM is copied on this Datastore.
---
---

(internal test :-  VM-> RHEL 7.2 with 2 disks, HW version-13, ext4 filesystem mounted in Linux with –discard option
	- virtual disk 2, presented as  /dev/sdb -> 100G thin vmdk from a thin 120G LU datastore.
	- virtual disk 3, /dev/sdc -> 120G RDM added to VM.
```bash
	mkfs.ext4 /dev/sdb1
	mount /dev/sdb1 /u02 -o discard
```
(both the datastores are on thin LUs) 

	
#### 2. Delete 1GB file from within Guest OS on both disks
---
---

Sample outputs from internal tests

##### BEFORE DELETE OF FILE:
###### a. Optional VMware script to check # of UNMAP I/Os
---

	[root@localhost:~] vsish /> get /vmkModules/vmfs3/auto_unmap/volumes/Thin_DS2_R800/properties
	Volume specific unmap information {
	   Volume Name                 :Thin_DS2_R800
	   FS Major Version            :24
	   Metadata Alignment          :4096
	   Allocation Unit/Blocksize   :1048576
	   Unmap granularity in File   :1048576
	   Volume: Unmap IOs           :6
	   Volume: Unmapped blocks     :324
	   Volume: Num wait cycles     :0
	   Volume: Num from scanning   :6
	   Volume: Num from heap pool  :5
	   Volume: Total num cycles    :57

###### *b. Check Capacity on datastore*
---

	[root@localhost:~] vmkfstools -Ph -v 1 /vmfs/volumes/Thin_DS2_R800/
	VMFS-6.81 (Raw Major Version: 24) file system spanning 1 partitions.
	File system label (if any): Thin_DS2_R800
	Mode: public ATS-only
	Capacity 119.8 GB, 93.1 GB available, file block size 1 MB, max supported file size 64 TB
	Volume Creation Time: Sun Jan 14 21:47:26 2001
	Files (max/free): 16384/16371
	Ptr Blocks (max/free): 0/0
	Sub Blocks (max/free): 16384/16363
	Secondary Ptr Blocks (max/free): 256/255
	File Blocks (overcommit/used/overcommit %): 0/27316/0
	Ptr Blocks  (overcommit/used/overcommit %): 0/0/0
	Sub Blocks  (overcommit/used/overcommit %): 0/21/0
	Large File Blocks (total/used/file block clusters): 240/0/240
	Volume Metadata size: 1504247808
	UUID: 3a621e6e-1f3492e8-29a5-6cc2172acba0
	Partitions spanned (on "lvm"): naa.60060e80072720000030272000000032:1
	Is Native Snapshot Capable: NO
	346292:VVOLLIB : VVolLibTracingExit:150: Successfully cleaned up the VVolLib tracing module
	OBJLIB-LIB: ObjLib cleanup done.
	WORKER: asyncOps=0 maxActiveOps=0 maxPending=0 maxCompleted=0
	
###### c. Check Capacity in Linux VM
---

	[root@localhost ~]# df -h
	Filesystem             Size  Used Avail Use% Mounted on
	/dev/mapper/rhel-root   23G  3.5G   19G  16% /
	devtmpfs               910M     0  910M   0% /dev
	tmpfs                  921M     0  921M   0% /dev/shm
	tmpfs                  921M  8.5M  912M   1% /run
	tmpfs                  921M     0  921M   0% /sys/fs/cgroup
	/dev/sda1              497M  125M  373M  25% /boot
	tmpfs                  185M     0  185M   0% /run/user/0
	/dev/sdb1               99G  1.1G   93G   2% /disk1
	/dev/sdc1              118G  1.1G  111G   1% /disk2
	
###### d. Delete files in guest OS
---

	[root@localhost ~]# rm /disk2/test2.zip 
	rm: remove regular file â€˜/disk2/test2.zipâ€™? y
	[root@localhost ~]# rm /disk1/test1.zip 
	rm: remove regular file â€˜/disk1/test1.zipâ€™? y
	[root@localhost ~]# 

##### AFTER DELETE OF FILE:
###### e. Optional: to check # of UNMAP I/Os
---

	[root@localhost:~] vsish  /> get /vmkModules/vmfs3/auto_unmap/volumes/Thin_DS2_R800/properties
	Volume specific unmap information {
	   Volume Name                 :Thin_DS2_R800
	   FS Major Version            :24
	   Metadata Alignment          :4096
	   Allocation Unit/Blocksize   :1048576
	   Unmap granularity in File   :1048576
	   Volume: Unmap IOs           :20
	   Volume: Unmapped blocks     :1187
	   Volume: Num wait cycles     :0
	   Volume: Num from scanning   :7
	   Volume: Num from heap pool  :8
	   Volume: Total num cycles    :68
	}

###### f. Check Capacity on datastore
---

	[root@localhost:~] vmkfstools -Ph -v 1 /vmfs/volumes/Thin_DS2_R800/
	VMFS-6.81 (Raw Major Version: 24) file system spanning 1 partitions.
	File system label (if any): Thin_DS2_R800
	Mode: public ATS-only
	Capacity 119.8 GB, 94.1 GB available, file block size 1 MB, max supported file size 64 TB
	Volume Creation Time: Sun Jan 14 21:47:26 2001
	Files (max/free): 16384/16371
	Ptr Blocks (max/free): 0/0
	Sub Blocks (max/free): 16384/16363
	Secondary Ptr Blocks (max/free): 256/255
	File Blocks (overcommit/used/overcommit %): 0/26268/0
	Ptr Blocks  (overcommit/used/overcommit %): 0/0/0
	Sub Blocks  (overcommit/used/overcommit %): 0/21/0
	Large File Blocks (total/used/file block clusters): 240/0/240
	Volume Metadata size: 1504247808
	UUID: 3a621e6e-1f3492e8-29a5-6cc2172acba0
	Partitions spanned (on "lvm"): naa.60060e80072720000030272000000032:1
	Is Native Snapshot Capable: NO
	346339:VVOLLIB : VVolLibTracingExit:150: Successfully cleaned up the VVolLib tracing module
	OBJLIB-LIB: ObjLib cleanup done.
	WORKER: asyncOps=0 maxActiveOps=0 maxPending=0 maxCompleted=0

###### g. Check Capacity in Linux VM
---

	[root@localhost ~]# df -h
	Filesystem             Size  Used Avail Use% Mounted on
	/dev/mapper/rhel-root   23G  3.5G   19G  16% /
	devtmpfs               910M     0  910M   0% /dev
	tmpfs                  921M     0  921M   0% /dev/shm
	tmpfs                  921M  8.5M  912M   1% /run
	tmpfs                  921M     0  921M   0% /sys/fs/cgroup
	/dev/sda1              497M  125M  373M  25% /boot
	tmpfs                  185M     0  185M   0% /run/user/0
	/dev/sdb1               99G   61M   94G   1% /disk1
	/dev/sdc1              118G   61M  112G   1% /disk2
