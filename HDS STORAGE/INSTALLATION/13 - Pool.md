### CREATE POOL(s)
---
---



#### CREATE POOL(s) - Storage Advisor Embedded
---

Set up spare drives: In the navigation bar, click **Others > Configure Spare Drives.**
![st_pool01.png](../../.images/st_pool01.png)

Create Pool:  In the navigation bar, click **Pools**.
![st_pool02.png](../../.images/st_pool02.png)


#### CREATE POOL(s) - SVP
---

**Multi-Tier Pool**: If main pool is tiered then create “Thin Image” pool for snapshots. Otherwise use “Dynamic Provisioning” pool for both DP V-Vols and snapshots and disable “Multi-Tier Pool”.
**Data Direct Mapping**: This is used for the migration from external volumes bigger than 4TB.
**Depletion Threshold**: Stop recording differential data of snapshots when it is exceeded.

![st_pool03.png](../../.images/st_pool03.png)
![st_pool04.png](../../.images/st_pool04.png)
![st_pool05.png](../../.images/st_pool05.png)

#### Increase Shared Memory if Pool creation fails (takes ~ 30/45 min)
---

![st_pool06.png](../../.images/st_pool06.png)
![st_pool07.png](../../.images/st_pool07.png)
![st_pool08.png](../../.images/st_pool08.png)
![st_pool09.png](../../.images/st_pool09.png)