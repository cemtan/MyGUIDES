### vSphere ESXi 6.5 Automated UNMAP
---
---



#### Introduction
---

In vSphere 6.5, there is now an automated UNMAP crawler mechanism for reclaiming dead or stranded space on VMFS datastores. This automatically reclaims stranded space due to 1) VM deletions or Storage vMotions or 2) In-Guest OS data deletions. Prior to vSphere 6.5, these were manual operations for administrators. 
Additional Background: Check UNMAP section @ https://storagehub.vmware.com/export_to_pdf/vsphere-6-5-storage 

	
#### Key Dependencies
---

- Use Thin provisioned VMDKs/RDMs back with thin provisioned LDEVs/LUs
- For Linux guest OS, use hardware version 13 or later to present SCSI-4
- For Windows 2012R2 OS, hardware version 1 or higher is fine 
- VMFS 6 datastores
- vSphere ESXi 6.5 (VMware ESXi 6.5.0 build-4564106 used internally)
- Storage: VSP G/F1x000 ( T-code Microcode: 80-05-46-00/80.)


#### Sections
---

- Initial Setup for Automated UNMAP on VSP G/F Storage
- TWO USE CASES for Automated UNMAP
	1. Observe automatic space reclaim after In-guest OS data deletion
	2. Observe automatic space reclaim after VM deletion
- FAQ
- Appendix for Support
