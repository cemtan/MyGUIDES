#### GET TOTAL SNAP SPACE
---
---


```bash
#!/bin/bash

INS="114"
ldevs="00:15:02 00:15:03 00:15:04 00:15:05 00:15:06"

SNAP_USED_POOL=0
SNAP_USED_TOTAL=0
SNAP_GARBAGE_TOTAL=0
NUM='^[0-9]+$'
for ldev in ${ldevs}; do
	r=""
	r=$(raidcom get ldev -ldev_id ${ldev} -I${INS})
	snap_used_pool=$(echo "${r}" | grep Snap_Used_Pool | awk -F" : " '{ print $2}')
	snap_used=$(echo "${r}" | grep SNAP_USED | awk -F" : " '{ print $2}')
	snap_garbage=$(echo "${r}" | grep -v DELETING | grep SNAP_GARBAGE | awk -F" : " '{ print $2}')
	 ${snap_used_pool} =~ ${NUM}  && SNAP_USED_POOL=$(( ${SNAP_USED_POOL} + ${snap_used_pool} ))
	 ${snap_used} =~ ${NUM}  && SNAP_USED_TOTAL=$(( ${SNAP_USED_TOTAL} + ${snap_used} ))
	 ${snap_garbage} =~ ${NUM}  && SNAP_GARBAGE_TOTAL=$(( ${SNAP_GARBAGE_TOTAL} + ${snap_garbage} ))    
done

SNAP_USED_POOL=$(( ${SNAP_USED_POOL} / 1024 ))
SNAP_USED_TOTAL=$(( ${SNAP_USED_TOTAL} / 1024 ))
SNAP_GARBAGE_TOTAL=$(( ${SNAP_GARBAGE_TOTAL} / 1024 ))


paste <(echo "SNAP USED POOL (GB)"$'\n'"${SNAP_USED_POOL}") \
		<(echo "SNAP USED TOTAL (GB)"$'\n'"${SNAP_USED_TOTAL}") \
		<(echo "SNAP GARBAGE TOTAL (GB)"$'\n'"${SNAP_GARBAGE_TOTAL}") | column -s $'\t' -t
```