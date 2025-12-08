#### CASCADE
---
---



**DP Pool must be used for cascade snapshot**
```bash
raidcom add ldev -pool 0 -ldev_id <LDEV> -capacity <CAP> -drs -request_id auto -capacity_saving compression -I<INST>

raidcom add snapshot -ldev_id 1B:74  5B:74 -pool 0 -snapshotgroup RO -snap_mode CTG cascade -I2
raidcom add snapshot -ldev_id 5B:74  4B:74 -pool 0 -snapshotgroup IFCLN -snap_mode CTG cascade  -I2
raidcom add snapshot -ldev_id 5B:74  6B:74 -pool 0 -snapshotgroup INFORMATICA -snap_mode CTG cascade  -I2
 
raidcom add snapshot -ldev_id 5B:C6 6B:C6 -pool 0 -snapshotgroup INFORMATICA -snap_mode cascade -I60
raidcom get snapshot -snapshotgroup RO  -check_status PAIR -time 900 -I60
raidcom modify snapshot -snapshotgroup RO  -snapshot_data split -I60
raidcom modify snapshot -snapshotgroup INFORMATICA  -snapshot_data split -I60
```
