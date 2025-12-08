#### FAQ
---
---



##### 1. When a VM-OS or ESX host do an unmap. will the box do a reclaim immediately or time scheduled (?? time)?
---

ESXi 6.5 host will issue the unmap asynchronously to the storage array. The space reclamation operates in the background on ESXi host (like a crawler process) and it frequency is based priority levels set per individual VMFS datastores. From the initial set of tests we have done, we have seen UNMAP being issued from ESXi anywhere from 30 seconds to 60 minutes after in-guest VM data deletion and sometimes longer (3+hours) for VM/virtual disk migration/deletion. The Hitachi Storage array will process that request so space reclamation completion time depends on resources available to process the request. VMware Engineering have acknowledged they are starting off conservatively but we are engaged with them to find a more dynamic approach in future release for those customers who may need a faster space reclaimation.

	
##### 2. Reclaim is a background process, are there impact counters available (CPU/IOPS/xFer/FEP) or best practices?
---

There are unmap counters with commands like esxtop and more with “vsish”, a VMware support tool script.  We are formulating some best practices and guidance as we review the initial set of tests but with ESXi being conservative, (25MB/sec), there should be no impact.

	
##### 3. Will reclaiming data stops or postponed when there is less ?? MP utilization available for that disk?
---

The process is treated as a lower class service to avoid impact to production I/O services. The data rate from ESXi is conservative so no contention problem for Hitachi VSP class storage

	
##### 4. An unmap command from ESX/VM for 10 pages, will this xFer 420MB (10 x 42MB page) of data over the SAN link or just 1 VAAI command for reclaim the 10 pages?
---

ESXi will send LBA range, not the actual page data. ESXi will issue unmap requests in increments on 1MB and multiples of 1MB. Hitachi VSP can do unmap as granular as 256KB. [Need to confirm if one/more requests for large block sizes but I believe one unmap command includes as much dead space reclaimation that crawler observed..I will verify this ]

	
##### 5. Follow up on the above question: When a datastore is replicated, will all reclaimed data sync or just the VAAI command to svol side and also reclaimed at the svol side?
---

Reclamation process zeros out the data which would get replicated/deduped but let me get consensus data with an engineering test with auto unmap

	
##### 6. With VVOL configuration, you create larger disks, is there a different (unmap) impact compare with the VMFS datastores?
---

With VVol , there is no UNMAP for activities like VM vMotion or VM / virtual disk deletion which would normally leave unreclaimed spece in a datastore. For VVol, each vmdk is its own mini-LUN so no space reclamation needed. For in-guest VM data deletion, it would behave the same as VMFS6 but we have not done that specific test yet.

	
##### 7. You can set discard or use fstrim in Linux, are there best practices? I suppose when you deleting and recreate data constantly it is useless to set an unmap option, but maybe also other combinations? 
---

I’m working with VMware to help capture some of these best practices. I hope to publish a blog on the topic but will share when I have draft content from VMware. We’ve used the –discard option in our internal tests.
