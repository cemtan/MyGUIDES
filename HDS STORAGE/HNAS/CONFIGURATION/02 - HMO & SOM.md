### HOST MODE OPTIONS & SYSTEM OPTION MODES
---
---


 
#### HOST MODE OPTIONS
---

##### HMO 7
Enabled on each host group being accessed by HNAS.

This mode is used to switch the setting of whether to return the Unit Attention response when adding a LUN; this will prevent the need to run "**scsi-refresh**" after presenting LUNs to HNAS; the newly presented LUNs should be auto-discovered.

From the SAM spec, the functionality is as follows: If the logical unit inventory changes for any reason (e.g., completion of initialization, removal of a logical unit, or creation of a logical unit), then the device server shall establish a unit attention condition for the initiator port associated with every I\_T nexus, with the additional sense code set to REPORTED LUNS DATA HAS CHANGED.
**Note**: HNAS will support this  bit in v12.5(and on). Enabling this bit when using earlier versions of HNAS is acceptable, but does not remove the requirement to run "**scsi-refresh**".


##### HMO 68
Enabled on each host group being accessed by HNAS.  This will allow the server to access information about the device that is required to support HDP page reclaim functionality using the UNMAP and WriteSame SCSI commands in order to determine the amount of vacated chunks to process as part of the command span-unmap-vacated-chunks.  Whilst the command to vacate chunks is not expected to be widely used, enabling HMO 68 is nonetheless required.


##### HMO 78 (GAD olduğu durumda)
Enabled on each host group being accessed by "remote" HNAS in a GAD solution. HNAS obtains the information from VPD Pg 0xE0, byte 4, to determine whether the SD is accessed from the local or remote array. For optimal performance the choice of local access path takes greater precedence than access to P-Vol. 

The cross path configuration in GAD environment is supported. In the configuration, if cross paths are active (prioritized), I/Os run on the cross paths so that I/O response performance degrades for the roundtrip working on the cross paths.

- VSP (F)Gx00, VSP G1000



#### SYSTEM OPTION MODES
---

##### SOM 896
Ensures that pages are formatted in the background rather than at allocation.  Setting this incorrectly will cause pages to be formatted upon allocation, incurring a performance penalty.

- HUS-VM, verify SOM 896 is OFF (this is the default) - note, the desired HUS VM functionality occurs as default; setting this bit to **ON** will disable the functionality.
- VSP, set SOM 896 to ON.
- VSP G1000/Gx00, verify SOM 896 is ON(this is the default) - note, the desired VSP G1000/Gx00 functionality occurs as default; setting this bit to **OFF** will disable the functionality.

 
##### SOM 901 (Tiering olduğu durumda)
By setting this mode to ON while the drive type of DT tier1 is SSD, page allocation of Tier Level ALL will not go to the next tier until the capacity reaches the limit without consideration of performance potential.  This makes sure that the pages are not demoted when the performance utilization on an SSD tier exceeds 60%. As the response times from the SSD are relatively good, the performance utilization on the SSD tier can exceed the 60% recommended limit.

- NA for HUS-VM


##### SOM 904 (Tiering olduğu durumda) (but not by default, be guided by GSC analysis)
This mode is used to reduce the load of drive so as to improve host I/O performance by reducing the amount of page migration per second (42MB/s) at tier relocation.  This option should be explored if an HNAS system experiences storage response time degradation in the environments with frequent relocation and rebalancing.

- NA for HUS-VM


##### SOM 917
This mode supports rebalancing to average page usage rate among parity groups or external volume groups in which pool volumes are defined to reduce drive workload of the parity groups or external volume groups.
