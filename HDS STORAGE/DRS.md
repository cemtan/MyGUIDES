#### DRS (Thin Image Advanced)
---



```ldev -> DRS enabled (supported by OPS 11)```

Before you use the capacity saving function, Dynamic Provisioning and dedupe and compression must be installed on the storage system. If you want to create a DRS-VOL and perform the DRS-VOL operations, Adaptive Data Reduction must be installed.

https://knowledge.hitachivantara.com/Documents/Management_Software/SVOS/9.8.7/Local_Replication/Thin_Image_Advanced/03_Configuring_Thin_Image_Advanced#missing-elem-id--GUID-CAF9AF57-0D91-41E3-89F0-D3975E962FF9

```bash
raidcom add ldev -ldev_id 0x010 -drs -capacity_saving compression -request_id auto -pool 1 -capacity 102400G

raidcom get ldev -ldev_id 0x010 -fx
Serial# : 500001
LDEV : 10
SL : 0
CL : 0
VOL_TYPE : OPEN-V-CVS
VOL_Capacity(BLK) : 214748364800
NUM_PORT : 0
PORTs :
F_POOLID : NONE
VOL_ATTR : CVS : HDP : DRS
```



#### Protector
---

```snap -> advanced -> cascade - fully provisioned - DRU enabled```
