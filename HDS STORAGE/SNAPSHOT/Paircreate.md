#### PAIRCREATE
---
---



##### SNAPHOT
---
```bash
raidcom add ldev -pool snap -ldev_id <LDEV> -capacity <CAP> -I<INST>
```
Edit horcm
```bash
paircreate -g <GROUP> -d <DISK> -vl -m grp <CTG> -IT<INST>
paircreate -g <GROUP> -d <DISK> -vl -IM<INST> -pid <POOL_ID>
```
Consistency group ID is displayed in the following windows

###### in HDvM - SN:
   ■ Consistency Groups tab in Local Replication window.
   ■ Consistency Group Properties window.
   ■ In CCI, use the raidcom get snapshot command to view the consistency group ID.

###### with raidcom
```bash
raidcom get snapshot -snapshotgroup <GROUP> -fx -format_time -I<INST>
```



##### CLONE
---
```bash
raidcom add ldev -pool <POOLID> -ldev_id <LDEV> -capacity <CAP> -I<INST>
```
Edit horcm
```bash
paircreate -g <GROUP> -d <DISK> -vl -m grp <CTG> -IS<INST>

paircreate -g HAVol_GAD -fg never 1 -vl -jq 0 -IH0
paircreate -g HAVol_DLT -f async -vl -jp 0 -js 0 -nocsus –IH1
paircreate -g HAVol_HUR -f async -vl -jp 0 -js 0 –IH0
```
###### For TC: 
```bash
paircreate –g <grp> -vl –fg <fence> <CTGID> –IH<HORCMinstance #>
```
###### For UR: 
```bash
paircreate –g <grp> -vl –f async –jp <journal id> –js <journalid> –IH<HORCM instance #>
```

###### For GAD: 
```bash
paircreate –g <grp> -vl –fg never –jq <quorum id> –IH<HORCM instance #>
```


