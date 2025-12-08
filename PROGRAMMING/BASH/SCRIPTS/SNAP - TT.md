#### SNAPSHOT - Turk Telekom
---
---



##### CREATING SNAP DISKS
---
```bash
#!/bin/bash

#SG1
n=4096
disk="137 138 140 141 142 143 144 145 551 564 565 566 567 608 609 610 611 612 670 709 710 721 722 723 724 725 726"
for d in $disk; do
	cap=$(raidcom get ldev -ldev_id $d -I40171 | grep _Capacity | awk '{print $3}')
	nam=$(raidcom get ldev -ldev_id $d -I40171 | grep LDEV_NAMING | awk '{print $3}')
	nnam=$nam"_S1"
	raidcom add ldev -pool 0 -ldev_id $n -capacity $cap -capacity_saving compression -I40171
	sleep 1
	raidcom modify ldev -ldev_id $n -ldev_name $nnam -I40171
	((n++))
done

#SG2
n=4128
disk="137 138 140 141 142 143 144 145 551 564 565 566 567 608 609 610 611 612 670 709 710 721 722 723 724 725 726"
for d in $disk; do
	cap=$(raidcom get ldev -ldev_id $d -I40171 | grep _Capacity | awk '{print $3}')
	nam=$(raidcom get ldev -ldev_id $d -I40171 | grep LDEV_NAMING | awk '{print $3}')
	nnam=$nam"_S2"
	raidcom add ldev -pool 0 -ldev_id $n -capacity $cap -capacity_saving compression -I40171
	sleep 1
	raidcom modify ldev -ldev_id $n -ldev_name $nnam -I40171
	((n++))
done
```


##### CREATING SNAPSHOT
---
```bash
#!/bin/bash

#SG1
disk="137 138 140 141 142 143 144 145 551 564 565 566 567 608 609 610 611 612 670 709 710 721 722 723 724 725 726"
for d in $disk; do
	raidcom add snapshot -snapshotgroup PYROAR00MDF0_SG1 -pool 0 -ldev_id $d -snap_mode CTG cascade -I40171
done
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG1 -check_status PAIR -time 7200 -I40171
raidcom modify snapshot -snapshotgroup PYROAR00MDF0_SG1 -snapshot_data create -I40171
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG1 -check_status PSUS -time 7200 -I40171

#SG2
disk="137 138 140 141 142 143 144 145 551 564 565 566 567 608 609 610 611 612 670 709 710 721 722 723 724 725 726"
for d in $disk; do
	raidcom add snapshot -snapshotgroup PYROAR00MDF0_SG2 -pool 0 -ldev_id $d -snap_mode CTG cascade -I40171
done
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG2 -check_status PAIR -time 7200 -I40171
raidcom modify snapshot -snapshotgroup PYROAR00MDF0_SG2 -snapshot_data create -I40171
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG2 -check_status PSUS -time 7200 -I40171
```


##### MAPPING
---
```bash
#!/bin/bash

#SG1
n=4096
disk="137 138 140 141 142 143 144 145 551 564 565 566 567 608 609 610 611 612 670 709 710 721 722 723 724 725 726"
for d in $disk; do
	raidcom map snapshot -snapshotgroup PYROAR00MDF0_SG1 -ldev_id $d $n -I40171
	((n++))
done

#SG2
n=4128
disk="137 138 140 141 142 143 144 145 551 564 565 566 567 608 609 610 611 612 670 709 710 721 722 723 724 725 726"
for d in $disk; do
	raidcom map snapshot -snapshotgroup PYROAR00MDF0_SG2 -ldev_id $d $n -I40171
	((n++))
done
```



##### RESYNCING SNAPSHOT
---
```bash
#!/bin/bash

#SG1
#horcmstart.sh 40171
#raidcom -login user password -I40171
n=4096
disk="137 138 140 141 142 143 144 145 551 564 565 566 567 608 609 610 611 612 670 709 710 721 722 723 724 725 726"
for d in $disk; do
	raidcom unmap snapshot -snapshotgroup PYROAR00MDF0_SG1 -ldev_id $d $n -I40171
	((n++))
done

echo "GET IF DISKS ARE UNMAPPED"
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG1 -format_time -I40171
echo "-------------------------------------------------------------------------------------------------------------------"
raidcom modify snapshot -snapshotgroup PYROAR00MDF0_SG1 -snapshot_data resync -I40171
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG1 -check_status PAIR -time 7200 -I40171
echo "GET IF SNAPS ARE PAIRED"
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG1 -format_time -I40171
echo "-------------------------------------------------------------------------------------------------------------------"
raidcom modify snapshot -snapshotgroup PYROAR00MDF0_SG1 -snapshot_data split -I40171
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG1 -check_status PSUS -time 7200 -I40171
echo "GET IF SNAPS ARE SPLITTED"
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG1 -format_time -I40171
echo "-------------------------------------------------------------------------------------------------------------------"
n=4096
for d in $disk; do
	raidcom map snapshot -snapshotgroup PYROAR00MDF0_SG1 -ldev_id $d $n -I40171
	((n++))
done

echo "GET IF DISKS ARE MAPPED"
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG1 -format_time -I40171

#SG2
#horcmstart.sh 40171
#raidcom -login user password -I40171
n=4128
disk="137 138 140 141 142 143 144 145 551 564 565 566 567 608 609 610 611 612 670 709 710 721 722 723 724 725 726"
for d in $disk; do
	raidcom unmap snapshot -snapshotgroup PYROAR00MDF0_SG2 -ldev_id $d $n -I40171
	((n++))
done

echo "GET IF DISKS ARE UNMAPPED"
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG2 -format_time -I40171
echo "-------------------------------------------------------------------------------------------------------------------"
raidcom modify snapshot -snapshotgroup PYROAR00MDF0_SG2 -snapshot_data resync -I40171
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG2 -check_status PAIR -time 7200 -I40171
echo "GET IF SNAPS ARE PAIRED"
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG2 -format_time -I40171
echo "-------------------------------------------------------------------------------------------------------------------"
raidcom modify snapshot -snapshotgroup PYROAR00MDF0_SG2 -snapshot_data split -I40171
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG2 -check_status PSUS -time 7200 -I40171
echo "GET IF SNAPS ARE SPLITTED"
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG2 -format_time -I40171
echo "-------------------------------------------------------------------------------------------------------------------"
n=4128
for d in $disk; do
	raidcom map snapshot -snapshotgroup PYROAR00MDF0_SG2 -ldev_id $d $n -I40171
	((n++))
done

echo "GET IF DISKS ARE MAPPED"
raidcom get snapshot -snapshotgroup PYROAR00MDF0_SG2 -format_time -I40171
```
