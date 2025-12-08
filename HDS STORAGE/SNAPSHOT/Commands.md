### COMMANDS
---
---



#### Create Snapshot → PAIR
	raidcom add snapshot
	raidcom modify snapshot -snapshot_data resync
	paircreate
	
#### Take Snapshot → PSUS
	raidcom modify snapshot -snapshot_data create
	raidcom modify snapshot -snapshot_data split
	pairsplit

#### Restore Snapshot
	raidcom modify snapshot -snapshot_data restore

#### Delete Snapshot
	raidcom delete snapshot -snapshotgroup <NAME> -I<INST>
	raidcom delete lun 
	raidcom delete ldev

#### Show Snapshots
	raidcom get snapshot
	raidcom get snapshot -snapshotgroup <NAME> -fx -format_time
	raidcom get snapshot -snapshotgroup <NAME> -check_status PAIR -time 7200

#### Scan All LDEVs
	raidscan -p CL1-C-1 -fw -I1
	
##### Example
	/HORCM/usr/bin/horcmstart.sh 201
	/HORCM/usr/bin/raidcom -login snap_user Hitachisnap1324 -I201
	# BSPR snapshot commands
	/HORCM/usr/bin/raidcom get snapshot -snapshotgroup SNAP_PIKACHU00MDR_BSPR@00K -I201 -fx -format_time
	/HORCM/usr/bin/raidcom modify snapshot -snapshotgroup SNAP_PIKACHU00MDR_BSPR@00K -snapshot_data resync -I201
	/HORCM/usr/bin/raidcom get snapshot -snapshotgroup SNAP_PIKACHU00MDR_BSPR@00K -check_status PAIR -time 7200  -I201


 
#### WORKFLOW
##### INITIAL SETUP
###### Create Snap Ldev (look for -pool snap option)
	raidcom add ldev -pool snap -ldev_id <LDEV_ID> -capacity <CAP> -capacity_saving deduplication_compression -I<INST> 

###### Present Lun to Host (keep them unmounted or offline)
	raidcom add lun -port <HOST_GROUP> -lun_id <LUN_ID> -ldev_id <LDEV_ID> -I<INST> 

###### Add Snapshot
	raidcom add snapshot -snapshotgroup <SNAPSHOT_GROUP> -ldev_id <PLDEV_ID> <SLDEV_ID> -snap_mode CTG cascade -pool <POOL_ID> -I<INST> 
	raidcom add snapshot -ldev_id <PLDEV_ID> -pool <POOL_ID> -snapshotgroup <SNAPSHOT_GROUP> -snap_mode CTG -I<INST> 
	
###### Split Snapshot (create or split)
	raidcom modify snapshot -snapshotgroup <SNAPSHOT_GROUP> -snapshot_data create -I<INST> 
	
###### Map Snaphot
	raidcom map snapshot -ldev_id <PLDEV_ID> <SLDEV_ID> -snapshotgroup <SNAPSHOT_GROUP> -I<INST> 

###### Mount or Make Online Disks on Host

##### RESYNC
###### Unmount or Make Offline Disks on Host

###### Unmap Snapshot
	raidcom unmap snapshot -ldev_id <PLDEV_ID>-snapshotgroup <SNAPSHOT_GROUP> -I<INST> 

###### Resync Snapshot (wait for PAIR)
	raidcom modify snapshot -snapshotgroup <SNAPSHOT_GROUP> -snapshot_data resync -I<INST>
	raidcom get snapshot -snapshotgroup <SNAPSHOT_GROUP> -check_status PAIR -time 7200 -I<INST>
	
###### Split Snapshot (wait for PSUS)
	raidcom modify snapshot -snapshotgroup <SNAPSHOT_GROUP> -snapshot_data split -I<INST>
	raidcom get snapshot -snapshotgroup <SNAPSHOT_GROUP> -check_status PSUS -time 7200 -I<INST>
	
###### Map Snaphot
	raidcom map snapshot -ldev_id <PLDEV_ID> <SLDEV_ID> -snapshotgroup <SNAPSHOT_GROUP> -I<INST> 

###### Mount or Make Online Disks on Host
	
```bash
if [ $? -eq 0 ]; then
   /HORCM/usr/bin/raidcom modify snapshot -snapshotgroup SNAP_PIKACHU00MDR_BSPR@00K -snapshot_data split -I201
   /HORCM/usr/bin/raidcom get snapshot -snapshotgroup SNAP_PIKACHU00MDR_BSPR@00K -check_status PSUS -time 7200  -I201

	if [ $? -eq 0 ]; then
			#/HORCM/usr/bin/raidcom get snapshot -snapshotgroup SNAP_PIKACHU00MDR_BSPR@00K -I201 -fx -format_time
			echo "Split Success"
	else
			echo "Split Error"
			exit 20
	fi
	raidcom add snapshot -snapshotgroup sg1 -ldev_id 509 528 -snap_mode CTG cascade -pool 0 -I1
else
	echo "Timeout Pair Re-Sync"
	exit 10
fi
```

	C:\HORCM\etc>raidcom get host_grp -port CL1-C-1 server1 -I1
	PORT   GID  GROUP_NAME                       Serial# HMD          HMO_BITs
	CL1-C    0  1C-G00                            540683 LINUX/IRIX
	CL1-C    1  server1                           540683 LINUX/IRIX
	
	C:\HORCM\etc>raidscan -p CL1-C-1 -fw -I1
	PORT# /ALPA/C,TID#, LU#.Num(LDEV#....)...P/S, Status,Fence, LDEV#,LUN-WWN
	CL1-C-1 /e1/ 4, 28,   0.1(509)...........SMPL  ----  ------ -----,Unknown
	CL1-C-1 /e1/ 4, 28,   1.1(397)...........P-VOL PAIR  NEVER    397,60060e8008ad9c000050ad9c0000018d
	CL1-C-1 /e1/ 4, 28,   2.1(398)...........P-VOL PAIR  NEVER    398,60060e8008ad9c000050ad9c0000018e
	CL1-C-1 /e1/ 4, 28,   3.1(399)...........P-VOL PAIR  NEVER    399,60060e8008ad9c000050ad9c0000018f
	CL1-C-1 /e1/ 4, 28,   4.1(400)...........P-VOL PAIR  NEVER    400,60060e8008ad9c000050ad9c00000190
	CL1-C-1 /e1/ 4, 28,   5.1(401)...........P-VOL PAIR  NEVER    401,60060e8008ad9c000050ad9c00000191
	CL1-C-1 /e1/ 4, 28,   6.1(402)...........P-VOL PAIR  NEVER    402,60060e8008ad9c000050ad9c00000192
	CL1-C-1 /e1/ 4, 28,   7.1(403)...........P-VOL PAIR  NEVER    403,60060e8008ad9c000050ad9c00000193
	CL1-C-1 /e1/ 4, 28,   8.1(404)...........P-VOL PAIR  NEVER    404,60060e8008ad9c000050ad9c00000194
	CL1-C-1 /e1/ 4, 28,   9.1(405)...........P-VOL PAIR  NEVER    405,60060e8008ad9c000050ad9c00000195
	CL1-C-1 /e1/ 4, 28,  10.1(406)...........P-VOL PAIR  NEVER    406,60060e8008ad9c000050ad9c00000196
	CL1-C-1 /e1/ 4, 28,  11.1(407)...........P-VOL PAIR  NEVER    407,60060e8008ad9c000050ad9c00000197
	CL1-C-1 /e1/ 4, 28,  12.1(408)...........P-VOL PAIR  NEVER    408,60060e8008ad9c000050ad9c00000198
	CL1-C-1 /e1/ 4, 28,  13.1(409)...........P-VOL PAIR  NEVER    409,60060e8008ad9c000050ad9c00000199
	CL1-C-1 /e1/ 4, 28,  14.1(410)...........P-VOL PAIR  NEVER    410,60060e8008ad9c000050ad9c0000019a
	CL1-C-1 /e1/ 4, 28,  15.1(411)...........P-VOL PAIR  NEVER    411,60060e8008ad9c000050ad9c0000019b
	CL1-C-1 /e1/ 4, 28,  16.1(412)...........P-VOL PAIR  NEVER    412,60060e8008ad9c000050ad9c0000019c
	CL1-C-1 /e1/ 4, 28,  17.1(510)...........SMPL  ----  ------ -----,60060e8008ad9c000050ad9c000001fe
	
	
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   509    3     529    0  100 G--A 64634be7
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              -     -      540683     -    -       -    -    - ---- -
	
	
	
	
	C:\HORCM\etc>raidcom get snapshot -ldev_id 397 -format_time -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 2023-05-15T11:23:50
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3     508    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3     507    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3     506    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3     505    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3     508    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3     507    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3     506    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3     505    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3     508    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3     507    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3     506    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3     505    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3     508    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3     507    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3     506    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3     505    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646raidcom add snapshot -snapshotgroup sg1 -ldev_id 509 528 -snap_mode CTG cascade -pool 0 -I1
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3     508    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3     507    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3     506    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3     505    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3     508    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3     507    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3     506    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3     505    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3     508    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3     507    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3     506    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3     505    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646raidcom add snapshot -snapshotgroup sg1 -ldev_id 509 528 -snap_mode CTG cascade -pool 0 -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 64621646
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 64621646
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -snap_group SG1 -I1
	raidcom: Arguments of an option is incorrect '-snap_group'
	raidcom: [EW_UNWOPT] Unknown option
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	raidcom: [EX_ENOOBJ] No such Object in the RAID
	
	C:\HORCM\etc>raidcom get snapshot -I1
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 510 511 -
	raidcom: [EW_INVOPT] Invalid option
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 510 511 --
	raidcom: [EW_INVOPT] Invalid option
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 510 511 -snapshotgroup sg1 -snap_group CTG cascade
	raidcom: Arguments of an option is incorrect '-snap_group'
	raidcom: [EW_UNWOPT] Unknown option
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 510 511 -snapshotgroup sg1 -snap_mode CTG cascade
	raidcom: [EX_ATTHOR] Can't be attached to HORC manager
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 510 511 -snapshotgroup sg1 -snap_mode CTG cascade -I1
	raidcom: [EX_CMDRJE] An order to the control/command device was rejected
	It was rejected due to SKEY=0x05, ASC=0x26, ASCQ=0x00, SSB=0x2E20,0x0000 on Serial#(540683)
	CAUSE : LDEV is not installed.
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 510 -snapshotgroup sg1 -snap_mode CTG cascade -I1
	raidcom: [EX_CMDRJE] An order to the control/command device was rejected
	It was rejected due to SKEY=0x05, ASC=0x26, ASCQ=0x00, SSB=0x2E00,0x9701 on Serial#(540683)
	CAUSE : There are not enough required input parameters.
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 510 -snapshotgroup sg1 -snap_mode CTG cascade -pool 0 -I1
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -ldev_id 510 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PAIR   540683   510    3       -    0  100 G--A -
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PAIR   540683   510    3       -    0  100 G--A -
	
	C:\HORCM\etc>raidcom modify snapshot -ldev_id -snapshot_data create -I1
	raidcom: An option or arguments of an option are incorrect '-ldev_id'
	raidcom: [EW_INVOPT] Invalid option
	
	C:\HORCM\etc>raidcom modify snapshot -ldev_id 510 -snapshot_data create -I1
	raidcom: [EX_ENOOBJ] No such Object in the RAID
	
	C:\HORCM\etc>raidcom modify snapshot -snapshotgroup sg1 -snapshot_data create -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -format-time -I1
	raidcom: Arguments of an option is incorrect '-format-time'
	raidcom: [EW_UNWOPT] Unknown option
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -format_time -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   510    3       -    0  100 G--A 2023-05-15T15:26:44
	
	C:\HORCM\etc>raidcom modify snapshot -snapshotgroup sg1 -snapshot_data resync -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -format_time -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PAIR   540683   510    3       -    0  100 G--A -
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -format_time -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PAIR   540683   510    3       -    0  100 G--A -
	
	C:\HORCM\etc>raidcom modify snapshot -snapshotgroup sg1 -snapshot_data create -I1
	
	C:\HORCM\etc>raidcom modify snapshot -ldev_id 510 -snapshot_data create -I1
	raidcom: [EX_ENOOBJ] No such Object in the RAID
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   510    3       -    0  100 G--A 64624f66
	
	C:\HORCM\etc>raidcom modify snapshot -snapshotgroup sg1 -snapshot_data resync -I1
	
	C:\HORCM\etc>raidcom modify snapshot -snapshotgroup sg1 -snapshot_data split -I1
	
	C:\HORCM\etc>raidcom modify snapshot -snapshotgroup sg1 -snapshot_data resync -I1
	
	C:\HORCM\etc>raidcom modify snapshot -snapshotgroup sg1 -snapshot_data split -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   510    3       -    0  100 G--A 64624f8a
	
	C:\HORCM\etc>raidcom delete snapshot -snapshotgroup sg1 -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	raidcom: [EX_ENOOBJ] No such Object in the RAID
	
	C:\HORCM\etc>raidcom get snapshot -I1
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 510 -snapshotgroup sg1 -snap_mode CTG cascade -pool 0 -I1
	
	C:\HORCM\etc>raidcom modify snapshot -ldev_id 510 -snapshot_data create -I1
	raidcom: [EX_ENOOBJ] No such Object in the RAID
	
	C:\HORCM\etc>raidcom modify snapshot -snapshotgroup sg1 -snapshot_data create -I1
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -format_time-I1
	raidcom: Arguments of an option is incorrect '-format_time-I1'
	raidcom: [EW_UNWOPT] Unknown option
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -format_time -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   510    3       -    0  100 G--A 2023-05-15T15:29:28
	
	C:\HORCM\etc>
	C:\HORCM\etc>raidcom get ldev -ldev_id 510 -I1
	Serial#  : 540683
	LDEV : 510 VIR_LDEV : 510
	SL : 0
	CL : 0
	VOL_TYPE : OPEN-V-CVS
	VOL_Capacity(BLK) : 41943040
	NUM_PORT : 4
	PORTs : CL2-D-1 17 server1 : CL1-D-1 17 server1 : CL2-C-1 17 server1 : CL1-C-1 17 server1
	F_POOLID : NONE
	VOL_ATTR : CVS : QS : HDP
	CMP : -
	EXP_SPACE : -
	B_POOLID : 0
	LDEV_NAMING : test
	STS : NML
	OPE_TYPE : NONE
	OPE_RATE : 100
	MP# : 6
	SSID : 0005
	Used_Block(BLK) : 86016
	FLA(MB) : Disable
	RSV(MB) : 0
	CSV_Status : ENABLED
	CSV_PROGRESS(%) : -
	CSV_Mode : DEDUP+COMPRESS
	COMPRESSION_ACCELERATION : ENABLED
	COMPRESSION_ACCELERATION_STATUS : ENABLED
	CSV_PROCESS_MODE : INLINE
	DEDUPLICATION_DATA : ENABLED
	ALUA : Disable
	RSGID : 1
	PWSV_S : -
	Snap_Used_Pool(MB) : 0
	CL_MIG : N
	SNAP_USED(MB) : 84
	SNAP_GARBAGE(MB) : 0
	DELETING_SNAP_GARBAGE : NONE
	DELETING_SNAP_GARBAGE(%) : -
	
	C:\HORCM\etc>raidcom get resource -I1
	RS_GROUP            RGID   stat      Lock_owner     Lock_host      Serial#
	meta_resource          0   Unlocked  -              -               540683
	vsm                    1   Unlocked  -              -               540683
	HDIDProvisioning       2   Unlocked  -              -               540683
	
	C:\HORCM\etc>raidcom get resource -I2
	User for Serial#[540797] : maintenance
	Password :
	RS_GROUP            RGID   stat      Lock_owner     Lock_host      Serial#
	meta_resource          0   Unlocked  -              -               540797
	vsm                    1   Unlocked  -              -               540797
	HDIDProvisioning       2   Unlocked  -              -               540797
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 510 -I2
	Serial#  : 540797
	LDEV : 510
	SL : -
	CL : -
	VOL_TYPE : NOT DEFINED
	SSID : 0005
	RSGID : 0
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 0x019c -I1
	Serial#  : 540683
	LDEV : 412 VIR_LDEV : 412
	SL : 0
	CL : 0
	VOL_TYPE : OPEN-V-CVS
	VOL_Capacity(BLK) : 2147483648
	NUM_PORT : 4
	PORTs : CL2-D-1 16 server1 : CL1-D-1 16 server1 : CL2-C-1 16 server1 : CL1-C-1 16 server1
	F_POOLID : NONE
	VOL_ATTR : CVS : HDP : GAD
	CMP : -
	EXP_SPACE : -
	B_POOLID : 0
	LDEV_NAMING : Server1_GAD_16
	STS : NML
	OPE_TYPE : NONE
	OPE_RATE : 100
	MP# : 3
	SSID : 0005
	Used_Block(BLK) : 1887707136
	FLA(MB) : Disable
	RSV(MB) : 0
	CSV_Status : ENABLED
	CSV_PROGRESS(%) : -
	CSV_Mode : DEDUP+COMPRESS
	COMPRESSION_ACCELERATION : ENABLED
	COMPRESSION_ACCELERATION_STATUS : ENABLED
	CSV_PROCESS_MODE : INLINE
	DEDUPLICATION_DATA : ENABLED
	ALUA : Enable
	RSGID : 1
	PWSV_S : -
	CL_MIG : N
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 0x019c -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   510    3       -    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              -     -      540683     -    -       -    -    - ---- -
	SG1@000                          -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1@000 -I1
	raidcom: [EX_ENOOBJ] No such Object in the RAID
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 646257a3
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 646257a3
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 646257a3
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 646257a3
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 646257a3
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   398    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   399    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   400    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   401    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   402    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   403    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   404    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   405    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   406    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   407    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   408    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   409    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   410    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   411    3       -    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   412    3       -    0  100 G--A 646257a3
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3     520    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   398    3     519    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   399    3     518    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   400    3     517    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   401    3     516    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   402    3     515    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   403    3     514    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   404    3     513    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   405    3     512    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   406    3     511    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   407    3     526    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   408    3     525    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   409    3     524    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   410    3     523    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   411    3     522    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   412    3     521    0  100 G--A 646257a3
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup SG1@000 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	SG1@000                          P-VOL PSUS   540683   397    3     520    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   398    3     519    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   399    3     518    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   400    3     517    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   401    3     516    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   402    3     515    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   403    3     514    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   404    3     513    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   405    3     512    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   406    3     511    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   407    3     526    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   408    3     525    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   409    3     524    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   410    3     523    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   411    3     522    0  100 G--A 646257a3
	SG1@000                          P-VOL PSUS   540683   412    3     521    0  100 G--A 646257a3
	
	C:\HORCM\etc>
	C:\HORCM\etc>
	C:\HORCM\etc>
	C:\HORCM\etc>raidcom get ldev -ldev_id 520 -I1
	Serial#  : 540683
	LDEV : 520 VIR_LDEV : 520
	SL : 0
	CL : 0
	VOL_TYPE : OPEN-V-CVS
	VOL_Capacity(BLK) : 2147483648
	NUM_PORT : 5
	PORTs : CL1-E-1 2038 HDIDProvisionedH : CL1-C-1 254 server1 : CL1-D-1 254 server1 : CL2-D-1 254 server1 : CL2-C-1 254 server1
	F_POOLID : NONE
	VOL_ATTR : CVS : QS : HDP
	CMP : -
	EXP_SPACE : -
	B_POOLID : 0
	S_POOLID : 0
	LDEV_NAMING : 00:01:8D_SNAP
	STS : NML
	OPE_TYPE : NONE
	OPE_RATE : 100
	MP# : 0
	SSID : 0009
	Used_Block(BLK) : 0
	FLA(MB) : Disable
	RSV(MB) : 0
	CSV_Status : DISABLED
	CSV_PROGRESS(%) : -
	CSV_Mode : DISABLED
	COMPRESSION_ACCELERATION : -
	COMPRESSION_ACCELERATION_STATUS : -
	CSV_PROCESS_MODE : -
	DEDUPLICATION_DATA : DISABLED
	ALUA : Disable
	RSGID : 1
	PWSV_S : -
	CL_MIG : N
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -format_time-I1
	raidcom: Arguments of an option is incorrect '-format_time-I1'
	raidcom: [EW_UNWOPT] Unknown option
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -format_time -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   510    3       -    0  100 G--A 2023-05-15T15:29:28
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 520 -I1
	Serial#  : 540683
	LDEV : 520
	SL : -
	CL : -
	VOL_TYPE : NOT DEFINED
	SSID : -
	RSGID : 0
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   510    3       -    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 510 -I2
	Serial#  : 540797
	LDEV : 510
	SL : -
	CL : -
	VOL_TYPE : NOT DEFINED
	SSID : 0005
	RSGID : 0
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 510 -I1
	Serial#  : 540683
	LDEV : 510 VIR_LDEV : 510
	SL : 0
	CL : 0
	VOL_TYPE : OPEN-V-CVS
	VOL_Capacity(BLK) : 41943040
	NUM_PORT : 4
	PORTs : CL2-D-1 17 server1 : CL1-D-1 17 server1 : CL2-C-1 17 server1 : CL1-C-1 17 server1
	F_POOLID : NONE
	VOL_ATTR : CVS : QS : HDP
	CMP : -
	EXP_SPACE : -
	B_POOLID : 0
	LDEV_NAMING : test
	STS : NML
	OPE_TYPE : NONE
	OPE_RATE : 100
	MP# : 6
	SSID : 0005
	Used_Block(BLK) : 86016
	FLA(MB) : Disable
	RSV(MB) : 0
	CSV_Status : ENABLED
	CSV_PROGRESS(%) : -
	CSV_Mode : DEDUP+COMPRESS
	COMPRESSION_ACCELERATION : ENABLED
	COMPRESSION_ACCELERATION_STATUS : ENABLED
	CSV_PROCESS_MODE : INLINE
	DEDUPLICATION_DATA : ENABLED
	ALUA : Enable
	RSGID : 1
	PWSV_S : -
	Snap_Used_Pool(MB) : 0
	CL_MIG : N
	SNAP_USED(MB) : 84
	SNAP_GARBAGE(MB) : 0
	DELETING_SNAP_GARBAGE : NONE
	DELETING_SNAP_GARBAGE(%) : -
	
	C:\HORCM\etc>raidcom map snapshot -ldev_id 510 527 -snapshotgroup sg1 -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom get snapshot -ldev_id 510 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 510 -I1
	Serial#  : 540683
	LDEV : 510 VIR_LDEV : 510
	SL : 0
	CL : 0
	VOL_TYPE : OPEN-V-CVS
	VOL_Capacity(BLK) : 41943040
	NUM_PORT : 4
	PORTs : CL2-D-1 17 server1 : CL1-D-1 17 server1 : CL2-C-1 17 server1 : CL1-C-1 17 server1
	F_POOLID : NONE
	VOL_ATTR : CVS : QS : HDP
	CMP : -
	EXP_SPACE : -
	B_POOLID : 0
	LDEV_NAMING : test
	STS : NML
	OPE_TYPE : NONE
	OPE_RATE : 100
	MP# : 6
	SSID : 0005
	Used_Block(BLK) : 86016
	FLA(MB) : Disable
	RSV(MB) : 0
	CSV_Status : ENABLED
	CSV_PROGRESS(%) : -
	CSV_Mode : DEDUP+COMPRESS
	COMPRESSION_ACCELERATION : ENABLED
	COMPRESSION_ACCELERATION_STATUS : ENABLED
	CSV_PROCESS_MODE : INLINE
	DEDUPLICATION_DATA : ENABLED
	ALUA : Enable
	RSGID : 1
	PWSV_S : -
	Snap_Used_Pool(MB) : 0
	CL_MIG : N
	SNAP_USED(MB) : 84
	SNAP_GARBAGE(MB) : 0
	DELETING_SNAP_GARBAGE : NONE
	DELETING_SNAP_GARBAGE(%) : -
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 527 -I1
	Serial#  : 540683
	LDEV : 527 VIR_LDEV : 65534
	SL : 0
	CL : 0
	VOL_TYPE : OPEN-V-CVS
	VOL_Capacity(BLK) : 41943040
	NUM_PORT : 0
	PORTs :
	F_POOLID : NONE
	VOL_ATTR : CVS : QS : HDP
	CMP : -
	EXP_SPACE : -
	B_POOLID : 0
	S_POOLID : 0
	LDEV_NAMING : test
	STS : NML
	OPE_TYPE : NONE
	OPE_RATE : 100
	MP# : 6
	SSID : 0009
	Used_Block(BLK) : 86016
	FLA(MB) : Disable
	RSV(MB) : 0
	CSV_Status : ENABLED
	CSV_PROGRESS(%) : -
	CSV_Mode : DEDUP+COMPRESS
	COMPRESSION_ACCELERATION : ENABLED
	COMPRESSION_ACCELERATION_STATUS : ENABLED
	CSV_PROCESS_MODE : INLINE
	DEDUPLICATION_DATA : ENABLED
	ALUA : Disable
	RSGID : 1
	PWSV_S : -
	CL_MIG : N
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom add snapshot -snapshotgroup sg1 -ldev_id 509 528 -I1
	raidcom: [EX_CMDRJE] An order to the control/command device was rejected
	It was rejected due to SKEY=0x05, ASC=0x26, ASCQ=0x00, SSB=0x2E00,0x9701 on Serial#(540683)
	CAUSE : There are not enough required input parameters.
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 510 -snapshotgroup sg1 -snap_mode CTG cascade -pool 0 -I1
	
	C:\HORCM\etc>raidcom add snapshot -snapshotgroup sg1 -ldev_id 509 528 -snap_mode CTG cascade -pool 0 -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PAIR   540683   509    3     528    0  100 G--A -
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom modify snapshot -snapshotgroup sg1 -ldev_id 509 -snapshot_data create -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   509    3     528    0  100 G--A 64634be7
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	
	C:\HORCM\etc>
	C:\HORCM\etc>
	C:\HORCM\etc>
	C:\HORCM\etc>
	C:\HORCM\etc>
	C:\HORCM\etc>
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	raidscan -p CL1-C-1 -fw -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   509    3     528    0  100 G--A 64634be7
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 528 -I1
	Serial#  : 540683
	LDEV : 528 VIR_LDEV : 65534
	SL : 0
	CL : 0
	VOL_TYPE : OPEN-V-CVS
	VOL_Capacity(BLK) : 41943040
	NUM_PORT : 4
	PORTs : CL1-C-1 18 server1 : CL1-D-1 18 server1 : CL2-C-1 18 server1 : CL2-D-1 18 server1
	F_POOLID : NONE
	VOL_ATTR : CVS : QS : HDP
	CMP : -
	EXP_SPACE : -
	B_POOLID : 0
	S_POOLID : 0
	LDEV_NAMING : test
	STS : NML
	OPE_TYPE : NONE
	OPE_RATE : 100
	MP# : 5
	SSID : 0009
	Used_Block(BLK) : 86016
	FLA(MB) : Disable
	RSV(MB) : 0
	CSV_Status : ENABLED
	CSV_PROGRESS(%) : -
	CSV_Mode : DEDUP+COMPRESS
	COMPRESSION_ACCELERATION : ENABLED
	COMPRESSION_ACCELERATION_STATUS : ENABLED
	CSV_PROCESS_MODE : INLINE
	DEDUPLICATION_DATA : ENABLED
	ALUA : Disable
	RSGID : 1
	PWSV_S : -
	CL_MIG : N
	
	C:\HORCM\etc>raidcom delete lun -ldev_id 528 -port CL1-C-1 CL1-D-1 CL2-C-1 CL2-D-1 server1 -I1
	raidcom: could not find the hstgrp# from CL1-D-1.
	raidcom: [EX_ENOOBJ] No such Object in the RAID
	
	C:\HORCM\etc>raidcom delete lun -ldev_id 528 -port CL1-C-1 server1 -I1
	raidcom: LUN 18(0x12) will be used for deleting.
	
	C:\HORCM\etc>raidcom delete lun -ldev_id 528 -port CL1-D-1 server1 -I1
	raidcom: LUN 18(0x12) will be used for deleting.
	
	C:\HORCM\etc>raidcom delete lun -ldev_id 528 -port CL2-D-1 server1 -I1
	raidcom: LUN 18(0x12) will be used for deleting.
	
	C:\HORCM\etc>raidcom delete lun -ldev_id 528 -port CL2-C-1 server1 -I1
	raidcom: LUN 18(0x12) will be used for deleting.
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 528 -I1
	Serial#  : 540683
	LDEV : 528 VIR_LDEV : 65534
	SL : 0
	CL : 0
	VOL_TYPE : OPEN-V-CVS
	VOL_Capacity(BLK) : 41943040
	NUM_PORT : 0
	PORTs :
	F_POOLID : NONE
	VOL_ATTR : CVS : QS : HDP
	CMP : -
	EXP_SPACE : -
	B_POOLID : 0
	S_POOLID : 0
	LDEV_NAMING : test
	STS : NML
	OPE_TYPE : NONE
	OPE_RATE : 100
	MP# : 5
	SSID : 0009
	Used_Block(BLK) : 86016
	FLA(MB) : Disable
	RSV(MB) : 0
	CSV_Status : ENABLED
	CSV_PROGRESS(%) : -
	CSV_Mode : DEDUP+COMPRESS
	COMPRESSION_ACCELERATION : ENABLED
	COMPRESSION_ACCELERATION_STATUS : ENABLED
	CSV_PROCESS_MODE : INLINE
	DEDUPLICATION_DATA : ENABLED
	ALUA : Disable
	RSGID : 1
	PWSV_S : -
	CL_MIG : N
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   509    3     528    0  100 G--A 64634be7
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom unmap snapshot -ldev_id 509 -snapshotgroup sg1 -I1
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   509    3       -    0  100 G--A 64634be7
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom delete ldev -ldev_id 528 -I1
	
	C:\HORCM\etc>raidcomm get ldev -ldev_id 528 -I1
	'raidcomm' is not recognized as an internal or external command,
	operable program or batch file.
	
	C:\HORCM\etc>raidcom get ldev -ldev_id 528 -I1
	Serial#  : 540683
	LDEV : 528 VIR_LDEV : 65534
	SL : -
	CL : -
	VOL_TYPE : NOT DEFINED
	SSID : 0009
	RSGID : 1
	
	C:\HORCM\etc>raidcom get host_grp -port CL1-C-1 -key server -allports -I1
	PORT   GID  GROUP_NAME                       Serial# SRVID SRV_NAME
	CL1-C    0  1C-G00                            540683     - -
	CL1-C    1  server1                           540683     - -
	
	C:\HORCM\etc>raidcom get host_grp -port CL1-C-1 server1 -key server -allports -I1
	PORT   GID  GROUP_NAME                       Serial# SRVID SRV_NAME
	CL1-C    0  1C-G00                            540683     - -
	CL1-C    1  server1                           540683     - -
	
	C:\HORCM\etc>raidcom get host_grp -port CL1-C-1 server1 -key server -I1
	PORT   GID  GROUP_NAME                       Serial# SRVID SRV_NAME
	CL1-C    0  1C-G00                            540683     - -
	CL1-C    1  server1                           540683     - -
	
	C:\HORCM\etc>raidcom get host_grp -port CL1-C-1 server1 -key detail -I1
	PORT   GID RGID GROUP_NAME                       Serial# HMD          HMO_BITs
	CL1-C    0    0 "1C-G00"                          540683 LINUX/IRIX   -
	CL1-C    1    1 "server1"                         540683 LINUX/IRIX   -
	CL1-C    2    1 -                                 540683 -            -
	CL1-C    3    1 -                                 540683 -            -
	CL1-C    4    1 -                                 540683 -            -
	CL1-C    5    1 -                                 540683 -            -
	CL1-C    6    1 -                                 540683 -            -
	CL1-C    7    1 -                                 540683 -            -
	CL1-C    8    1 -                                 540683 -            -
	CL1-C    9    1 -                                 540683 -            -
	CL1-C   10    1 -                                 540683 -            -
	CL1-C   11    0 -                                 540683 -            -
	CL1-C   12    0 -                                 540683 -            -
	CL1-C   13    0 -                                 540683 -            -
	CL1-C   14    0 -                                 540683 -            -
	CL1-C   15    0 -                                 540683 -            -
	CL1-C   16    0 -                                 540683 -            -
	CL1-C   17    0 -                                 540683 -            -
	CL1-C   18    0 -                                 540683 -            -
	CL1-C   19    0 -                                 540683 -            -
	CL1-C   20    0 -                                 540683 -            -
	CL1-C   21    0 -                                 540683 -            -
	CL1-C   22    0 -                                 540683 -            -
	CL1-C   23    0 -                                 540683 -            -
	CL1-C   24    0 -                                 540683 -            -
	CL1-C   25    0 -                                 540683 -            -
	CL1-C   26    0 -                                 540683 -            -
	CL1-C   27    0 -                                 540683 -            -
	CL1-C   28    0 -                                 540683 -            -
	CL1-C   29    0 -                                 540683 -            -
	CL1-C   30    0 -                                 540683 -            -
	CL1-C   31    0 -                                 540683 -            -
	CL1-C   32    0 -                                 540683 -            -
	CL1-C   33    0 -                                 540683 -            -
	CL1-C   34    0 -                                 540683 -            -
	CL1-C   35    0 -                                 540683 -            -
	CL1-C   36    0 -                                 540683 -            -
	CL1-C   37    0 -                                 540683 -            -
	CL1-C   38    0 -                                 540683 -            -
	CL1-C   39    0 -                                 540683 -            -
	CL1-C   40    0 -                                 540683 -            -
	CL1-C   41    0 -                                 540683 -            -
	CL1-C   42    0 -                                 540683 -            -
	CL1-C   43    0 -                                 540683 -            -
	CL1-C   44    0 -                                 540683 -            -
	CL1-C   45    0 -                                 540683 -            -
	CL1-C   46    0 -                                 540683 -            -
	CL1-C   47    0 -                                 540683 -            -
	CL1-C   48    0 -                                 540683 -            -
	CL1-C   49    0 -                                 540683 -            -
	CL1-C   50    0 -                                 540683 -            -
	CL1-C   51    0 -                                 540683 -            -
	CL1-C   52    0 -                                 540683 -            -
	CL1-C   53    0 -                                 540683 -            -
	CL1-C   54    0 -                                 540683 -            -
	CL1-C   55    0 -                                 540683 -            -
	CL1-C   56    0 -                                 540683 -            -
	CL1-C   57    0 -                                 540683 -            -
	CL1-C   58    0 -                                 540683 -            -
	CL1-C   59    0 -                                 540683 -            -
	CL1-C   60    0 -                                 540683 -            -
	CL1-C   61    0 -                                 540683 -            -
	CL1-C   62    0 -                                 540683 -            -
	CL1-C   63    0 -                                 540683 -            -
	CL1-C   64    0 -                                 540683 -            -
	CL1-C   65    0 -                                 540683 -            -
	CL1-C   66    0 -                                 540683 -            -
	CL1-C   67    0 -                                 540683 -            -
	CL1-C   68    0 -                                 540683 -            -
	CL1-C   69    0 -                                 540683 -            -
	CL1-C   70    0 -                                 540683 -            -
	CL1-C   71    0 -                                 540683 -            -
	CL1-C   72    0 -                                 540683 -            -
	CL1-C   73    0 -                                 540683 -            -
	CL1-C   74    0 -                                 540683 -            -
	CL1-C   75    0 -                                 540683 -            -
	CL1-C   76    0 -                                 540683 -            -
	CL1-C   77    0 -                                 540683 -            -
	CL1-C   78    0 -                                 540683 -            -
	CL1-C   79    0 -                                 540683 -            -
	CL1-C   80    0 -                                 540683 -            -
	CL1-C   81    0 -                                 540683 -            -
	CL1-C   82    0 -                                 540683 -            -
	CL1-C   83    0 -                                 540683 -            -
	CL1-C   84    0 -                                 540683 -            -
	CL1-C   85    0 -                                 540683 -            -
	CL1-C   86    0 -                                 540683 -            -
	CL1-C   87    0 -                                 540683 -            -
	CL1-C   88    0 -                                 540683 -            -
	CL1-C   89    0 -                                 540683 -            -
	CL1-C   90    0 -                                 540683 -            -
	CL1-C   91    0 -                                 540683 -            -
	CL1-C   92    0 -                                 540683 -            -
	CL1-C   93    0 -                                 540683 -            -
	CL1-C   94    0 -                                 540683 -            -
	CL1-C   95    0 -                                 540683 -            -
	CL1-C   96    0 -                                 540683 -            -
	CL1-C   97    0 -                                 540683 -            -
	CL1-C   98    0 -                                 540683 -            -
	CL1-C   99    0 -                                 540683 -            -
	CL1-C  100    0 -                                 540683 -            -
	CL1-C  101    0 -                                 540683 -            -
	CL1-C  102    0 -                                 540683 -            -
	CL1-C  103    0 -                                 540683 -            -
	CL1-C  104    0 -                                 540683 -            -
	CL1-C  105    0 -                                 540683 -            -
	CL1-C  106    0 -                                 540683 -            -
	CL1-C  107    0 -                                 540683 -            -
	CL1-C  108    0 -                                 540683 -            -
	CL1-C  109    0 -                                 540683 -            -
	CL1-C  110    0 -                                 540683 -            -
	CL1-C  111    0 -                                 540683 -            -
	CL1-C  112    0 -                                 540683 -            -
	CL1-C  113    0 -                                 540683 -            -
	CL1-C  114    0 -                                 540683 -            -
	CL1-C  115    0 -                                 540683 -            -
	CL1-C  116    0 -                                 540683 -            -
	CL1-C  117    0 -                                 540683 -            -
	CL1-C  118    0 -                                 540683 -            -
	CL1-C  119    0 -                                 540683 -            -
	CL1-C  120    0 -                                 540683 -            -
	CL1-C  121    0 -                                 540683 -            -
	CL1-C  122    0 -                                 540683 -            -
	CL1-C  123    0 -                                 540683 -            -
	CL1-C  124    0 -                                 540683 -            -
	CL1-C  125    0 -                                 540683 -            -
	CL1-C  126    0 -                                 540683 -            -
	CL1-C  127    0 -                                 540683 -            -
	CL1-C  128    0 -                                 540683 -            -
	CL1-C  129    0 -                                 540683 -            -
	CL1-C  130    0 -                                 540683 -            -
	CL1-C  131    0 -                                 540683 -            -
	CL1-C  132    0 -                                 540683 -            -
	CL1-C  133    0 -                                 540683 -            -
	CL1-C  134    0 -                                 540683 -            -
	CL1-C  135    0 -                                 540683 -            -
	CL1-C  136    0 -                                 540683 -            -
	CL1-C  137    0 -                                 540683 -            -
	CL1-C  138    0 -                                 540683 -            -
	CL1-C  139    0 -                                 540683 -            -
	CL1-C  140    0 -                                 540683 -            -
	CL1-C  141    0 -                                 540683 -            -
	CL1-C  142    0 -                                 540683 -            -
	CL1-C  143    0 -                                 540683 -            -
	CL1-C  144    0 -                                 540683 -            -
	CL1-C  145    0 -                                 540683 -            -
	CL1-C  146    0 -                                 540683 -            -
	CL1-C  147    0 -                                 540683 -            -
	CL1-C  148    0 -                                 540683 -            -
	CL1-C  149    0 -                                 540683 -            -
	CL1-C  150    0 -                                 540683 -            -
	CL1-C  151    0 -                                 540683 -            -
	CL1-C  152    0 -                                 540683 -            -
	CL1-C  153    0 -                                 540683 -            -
	CL1-C  154    0 -                                 540683 -            -
	CL1-C  155    0 -                                 540683 -            -
	CL1-C  156    0 -                                 540683 -            -
	CL1-C  157    0 -                                 540683 -            -
	CL1-C  158    0 -                                 540683 -            -
	CL1-C  159    0 -                                 540683 -            -
	CL1-C  160    0 -                                 540683 -            -
	CL1-C  161    0 -                                 540683 -            -
	CL1-C  162    0 -                                 540683 -            -
	CL1-C  163    0 -                                 540683 -            -
	CL1-C  164    0 -                                 540683 -            -
	CL1-C  165    0 -                                 540683 -            -
	CL1-C  166    0 -                                 540683 -            -
	CL1-C  167    0 -                                 540683 -            -
	CL1-C  168    0 -                                 540683 -            -
	CL1-C  169    0 -                                 540683 -            -
	CL1-C  170    0 -                                 540683 -            -
	CL1-C  171    0 -                                 540683 -            -
	CL1-C  172    0 -                                 540683 -            -
	CL1-C  173    0 -                                 540683 -            -
	CL1-C  174    0 -                                 540683 -            -
	CL1-C  175    0 -                                 540683 -            -
	CL1-C  176    0 -                                 540683 -            -
	CL1-C  177    0 -                                 540683 -            -
	CL1-C  178    0 -                                 540683 -            -
	CL1-C  179    0 -                                 540683 -            -
	CL1-C  180    0 -                                 540683 -            -
	CL1-C  181    0 -                                 540683 -            -
	CL1-C  182    0 -                                 540683 -            -
	CL1-C  183    0 -                                 540683 -            -
	CL1-C  184    0 -                                 540683 -            -
	CL1-C  185    0 -                                 540683 -            -
	CL1-C  186    0 -                                 540683 -            -
	CL1-C  187    0 -                                 540683 -            -
	CL1-C  188    0 -                                 540683 -            -
	CL1-C  189    0 -                                 540683 -            -
	CL1-C  190    0 -                                 540683 -            -
	CL1-C  191    0 -                                 540683 -            -
	CL1-C  192    0 -                                 540683 -            -
	CL1-C  193    0 -                                 540683 -            -
	CL1-C  194    0 -                                 540683 -            -
	CL1-C  195    0 -                                 540683 -            -
	CL1-C  196    0 -                                 540683 -            -
	CL1-C  197    0 -                                 540683 -            -
	CL1-C  198    0 -                                 540683 -            -
	CL1-C  199    0 -                                 540683 -            -
	CL1-C  200    0 -                                 540683 -            -
	CL1-C  201    0 -                                 540683 -            -
	CL1-C  202    0 -                                 540683 -            -
	CL1-C  203    0 -                                 540683 -            -
	CL1-C  204    0 -                                 540683 -            -
	CL1-C  205    0 -                                 540683 -            -
	CL1-C  206    0 -                                 540683 -            -
	CL1-C  207    0 -                                 540683 -            -
	CL1-C  208    0 -                                 540683 -            -
	CL1-C  209    0 -                                 540683 -            -
	CL1-C  210    0 -                                 540683 -            -
	CL1-C  211    0 -                                 540683 -            -
	CL1-C  212    0 -                                 540683 -            -
	CL1-C  213    0 -                                 540683 -            -
	CL1-C  214    0 -                                 540683 -            -
	CL1-C  215    0 -                                 540683 -            -
	CL1-C  216    0 -                                 540683 -            -
	CL1-C  217    0 -                                 540683 -            -
	CL1-C  218    0 -                                 540683 -            -
	CL1-C  219    0 -                                 540683 -            -
	CL1-C  220    0 -                                 540683 -            -
	CL1-C  221    0 -                                 540683 -            -
	CL1-C  222    0 -                                 540683 -            -
	CL1-C  223    0 -                                 540683 -            -
	CL1-C  224    0 -                                 540683 -            -
	CL1-C  225    0 -                                 540683 -            -
	CL1-C  226    0 -                                 540683 -            -
	CL1-C  227    0 -                                 540683 -            -
	CL1-C  228    0 -                                 540683 -            -
	CL1-C  229    0 -                                 540683 -            -
	CL1-C  230    0 -                                 540683 -            -
	CL1-C  231    0 -                                 540683 -            -
	CL1-C  232    0 -                                 540683 -            -
	CL1-C  233    0 -                                 540683 -            -
	CL1-C  234    0 -                                 540683 -            -
	CL1-C  235    0 -                                 540683 -            -
	CL1-C  236    0 -                                 540683 -            -
	CL1-C  237    0 -                                 540683 -            -
	CL1-C  238    0 -                                 540683 -            -
	CL1-C  239    0 -                                 540683 -            -
	CL1-C  240    0 -                                 540683 -            -
	CL1-C  241    0 -                                 540683 -            -
	CL1-C  242    0 -                                 540683 -            -
	CL1-C  243    0 -                                 540683 -            -
	CL1-C  244    0 -                                 540683 -            -
	CL1-C  245    0 -                                 540683 -            -
	CL1-C  246    0 -                                 540683 -            -
	CL1-C  247    0 -                                 540683 -            -
	CL1-C  248    0 -                                 540683 -            -
	CL1-C  249    0 -                                 540683 -            -
	CL1-C  250    0 -                                 540683 -            -
	CL1-C  251    0 -                                 540683 -            -
	CL1-C  252    0 -                                 540683 -            -
	CL1-C  253    0 -                                 540683 -            -
	CL1-C  254    0 -                                 540683 -            -
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   509    3       -    0  100 G--A 64634be7
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom get snapshot -ldev_id 509 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   509    3       -    0  100 G--A 64634be7
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 529 -snapshotgroup sg2 -I1
	raidcom: [EX_CMDRJE] An order to the control/command device was rejected
	It was rejected due to SKEY=0x05, ASC=0x26, ASCQ=0x00, SSB=0x2E00,0x9701 on Serial#(540683)
	CAUSE : There are not enough required input parameters.
	
	C:\HORCM\etc>raidcom add snapshot -ldev_id 529 -snapshotgroup sg2 -pool 0 -snap_mode CTG cascade -I1
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              -     -      540683     -    -       -    -    - ---- -
	sg2                              -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg2 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg2                              P-VOL PAIR   540683   529    3       -    0  100 G--A -
	
	C:\HORCM\etc>raidcom delete snapshot -snapshotgroup sg2 -I1
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -ldev_id 529 -I1
	
	C:\HORCM\etc>raidcom map snapshot -ldev_id 509 529 -snapshotgroup sg1 -I1
	
	C:\HORCM\etc>raidcom get snapshot -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              -     -      540683     -    -       -    -    - ---- -
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   509    3     529    0  100 G--A 64634be7
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
	
	C:\HORCM\etc>raidcom unmap snapshot -ldev_id 529 -snapshotgroup sg1 -I1
	raidcom: [EX_ENOOBJ] No such Object in the RAID
	
	C:\HORCM\etc>raidcom get snapshot -snapshotgroup sg1 -I1
	SnapShot_name                    P/S   STAT  Serial# LDEV#  MU# P-LDEV#  PID    % MODE SPLT-TIME
	sg1                              P-VOL PSUS   540683   509    3     529    0  100 G--A 64634be7
	sg1                              P-VOL PSUS   540683   510    3     527    0  100 G--A 64624fd8
