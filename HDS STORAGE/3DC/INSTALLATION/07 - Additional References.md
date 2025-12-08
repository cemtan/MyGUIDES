### ADDITIONAL REFERENCES
---
---



#### 1. CREATE COMMAND DEVICES
---

	- 3F01 for opscenter CCI (user horcm)
	- 3F02 for analyzer (user probe)
	- 3F03 for protector (user protector)

##### 1.Create LDEVs
	raidcom add ldev -pool 0 -ldev_id 0x3F01 -capacity 1G -IH0
	raidcom add ldev -pool 0 -ldev_id 0x3F02 -capacity 1G -IH0
	raidcom add ldev -pool 0 -ldev_id 0x3F03 -capacity 1G -IH0
	raidcom add ldev -pool 0 -ldev_id 0x3F01 -capacity 1G -IH1
	raidcom add ldev -pool 0 -ldev_id 0x3F02 -capacity 1G -IH1
	raidcom add ldev -pool 0 -ldev_id 0x3F03 -capacity 1G -IH1
	raidcom modify ldev -ldev_id 0x3F01 -command_device y 2 -IH0
	raidcom modify ldev -ldev_id 0x3F01 -ldev_name CMD1_OPS -IH0
	raidcom modify ldev -ldev_id 0x3F02 -command_device y 2 -IH0
	raidcom modify ldev -ldev_id 0x3F02 -ldev_name CMD2_Probe -IH0
	raidcom modify ldev -ldev_id 0x3F03 -command_device y 2 -IH0
	raidcom modify ldev -ldev_id 0x3F03 -ldev_name CMD3_Protector -IH0
	raidcom modify ldev -ldev_id 0x3F01 -command_device y 2 -IH1
	raidcom modify ldev -ldev_id 0x3F01 -ldev_name CMD2_OPS -IH1
	raidcom modify ldev -ldev_id 0x3F02 -command_device y 2 -IH1
	raidcom modify ldev -ldev_id 0x3F02 -ldev_name CMD2_Probe -IH1
	raidcom modify ldev -ldev_id 0x3F03 -command_device y 2 -IH1
	raidcom modify ldev -ldev_id 0x3F03 -ldev_name CMD3_Protector -IH1

Check LDEVs

	raidcom get ldev -ldev_id 0x3F01 -key naa -IH0
	raidcom get ldev -ldev_id 0x3F02 -key naa -IH0
	raidcom get ldev -ldev_id 0x3F03 -key naa -IH0
	raidcom get ldev -ldev_id 0x3F01 -key naa -IH1
	raidcom get ldev -ldev_id 0x3F02 -key naa -IH1
	raidcom get ldev -ldev_id 0x3F03 -key naa -IH1

##### 2. Add LUNS
	raidcom add lun -port CL1-A-1 -lun_id 128 -ldev_id 0x3F01 -IH0
	raidcom add lun -port CL6-A-1 -lun_id 128 -ldev_id 0x3F01 -IH0
	raidcom add lun -port CL3-B-1 -lun_id 128 -ldev_id 0x3F01 -IH0
	raidcom add lun -port CL8-B-1 -lun_id 128 -ldev_id 0x3F01 -IH0
	raidcom add lun -port CL1-A-1 -lun_id 128 -ldev_id 0x3F01 -IH1
	raidcom add lun -port CL6-A-1 -lun_id 128 -ldev_id 0x3F01 -IH1
	raidcom add lun -port CL3-B-1 -lun_id 128 -ldev_id 0x3F01 -IH1
	raidcom add lun -port CL8-B-1 -lun_id 128 -ldev_id 0x3F01 -IH1
	
	raidcom add lun -port CL1-A-1 -lun_id 129 -ldev_id 0x3F02 -IH0
	raidcom add lun -port CL6-A-1 -lun_id 129 -ldev_id 0x3F02 -IH0
	raidcom add lun -port CL3-B-1 -lun_id 129 -ldev_id 0x3F02 -IH0
	raidcom add lun -port CL8-B-1 -lun_id 129 -ldev_id 0x3F02 -IH0
	raidcom add lun -port CL1-A-1 -lun_id 129 -ldev_id 0x3F02 -IH1
	raidcom add lun -port CL6-A-1 -lun_id 129 -ldev_id 0x3F02 -IH1
	raidcom add lun -port CL3-B-1 -lun_id 129 -ldev_id 0x3F02 -IH1
	raidcom add lun -port CL8-B-1 -lun_id 129 -ldev_id 0x3F02 -IH1
	
	raidcom add lun -port CL1-A-1 -lun_id 130 -ldev_id 0x3F03 -IH0
	raidcom add lun -port CL6-A-1 -lun_id 130 -ldev_id 0x3F03 -IH0
	raidcom add lun -port CL3-B-1 -lun_id 130 -ldev_id 0x3F03 -IH0
	raidcom add lun -port CL8-B-1 -lun_id 130 -ldev_id 0x3F03 -IH0
	raidcom add lun -port CL1-A-1 -lun_id 130 -ldev_id 0x3F03 -IH1
	raidcom add lun -port CL6-A-1 -lun_id 130 -ldev_id 0x3F03 -IH1
	raidcom add lun -port CL3-B-1 -lun_id 130 -ldev_id 0x3F03 -IH1
	raidcom add lun -port CL8-B-1 -lun_id 130 -ldev_id 0x3F03 -IH1

Check LUNs

	raidcom get lun -port CL1-A-1 -fx -IH0
	raidcom get lun -port CL6-A-1 -fx -IH0
	raidcom get lun -port CL3-B-1 -fx -IH0
	raidcom get lun -port CL8-B-1 -fx -IH0
	raidcom get lun -port CL1-A-1 -fx -IH1
	raidcom get lun -port CL6-A-1 -fx -IH1
	raidcom get lun -port CL3-B-1 -fx -IH1
	raidcom get lun -port CL8-B-1 -fx -IH1



#### 2. REMOVE COMMAND DEVICES
---
```bash
PORTS="CL1-A-1
CL6-A-1
CL3-B-1
CL8-B-1"

while read PORT; do
	for LDEV in "0x3F01 0x3F02 0x3F03"; do
		raidcom delete lun -port ${PORT} -ldev_id ${LDEV} -IH0
		raidcom delete lun -port ${PORT} -ldev_id ${LDEV} -IH1
	done
done < <(echo "${PORTS}")
for LDEV in "0x3F01 0x3F02 0x3F03"; do
	raidcom delete ldev -ldev_id ${LDEV} -I0
	raidcom delete ldev -ldev_id ${LDEV} -I1
done

```
```bash
PORTS="CL1-A-1
CL6-A-1
CL3-B-1
CL8-B-1"

LDEVS="0x3F01
0x3F02
0x3F03"

while read PORT; do
	while read LDEV; do
		raidcom delete lun -port ${PORT} -ldev_id ${LDEV} -IH0
	done < <(echo "${LDEVS}")
done < <(echo "${PORTS}")

while read LDEV; do
	raidcom delete ldev -ldev_id ${LDEV} -I0
done < <(echo "${LDEVS}")
```


#### 3. MOVE LDEVs BACK to META RESOURCE
---
```bash
for i in $(seq 0 512); do
	raidcom unmap resource -ldev_id ${i} -virtual_ldev_id ${i} -IH0
	raidcom add resource -resource_name meta_resource -ldev_id ${i} -IH0
	raidcom map resource -ldev_id ${i} -virtual_ldev_id ${i} -IH0
	raidcom unmap resource -ldev_id ${i} -virtual_ldev_id reserve -IH1
	raidcom add resource -resource_name meta_resource -ldev_id ${i} -IH1
	raidcom map resource -ldev_id ${i} -virtual_ldev_id ${i} -IH1
done
```


#### 4. GET DRIVE, LDEV, PARITY GROUP CONFIG
---
```bash
raidcom get ldev -ldev_list defined -IH0
raidcom get ldev -ldev_list defined -IH1
raidcom get drive -IH0
raidcom get drive -IH1
raidcom get parity_grp -IH0
raidcom get parity_grp -IH1

for i in $(seq 0 130); do
	echo "LDEV ${i}:  $(raidcom get ldev -ldev_id ${i} -IH0 | grep VOL_Capacity)”
done

for i in $(seq 0 130); do
	echo "LDEV ${i}:  $(raidcom get ldev -ldev_id ${i} -IH1 | grep VOL_Capacity)”
done
```


#### 5. DELETE POOL & RELATED LDEVs
---
```bash
raidcom delete pool -pool 0 -IH0
raidcom delete pool -pool 0 -IH1
raidcom delete pool -pool 0 -IH2

for LDEV in $(seq 0 130); do
	raidcom delete ldev -ldev_id ${LDEV} -IH0
	raidcom delete ldev -ldev_id ${LDEV} -IH1
	raidcom delete ldev -ldev_id ${LDEV} -IH2
done
```


#### 6. CREATE BASIC LDEVs
---
```bash
CAP="6420976108"
VSLD=0x3000
VELD=64
VSLD=$(echo $((${VSLD})))
VELD=$(expr ${VSLD} \+ ${VELD} \-1)
for i in $(seq ${VSLD} ${VELD}); do
	raidcom add ldev -parity_grp_id 1-1 -ldev_id ${i} -capacity ${CAP} -IH0
	raidcom add ldev -parity_grp_id 1-1 -ldev_id ${i} -capacity ${CAP} -IH1
	raidcom add ldev -parity_grp_id 1-1 -ldev_id ${i} -capacity ${CAP} -IH2
done
raidcom get parity_grp -I0

VSLD=0x3040
VSLD=$(echo $((${VSLD})))
VELD=$(expr ${VSLD} \+ ${VELD} \-1)
for i in $(seq ${VSLD} ${VELD}); do
	raidcom add ldev -parity_grp_id 1-2 -ldev_id ${i} -capacity ${CAP} -IH0
	raidcom add ldev -parity_grp_id 1-2 -ldev_id ${i} -capacity ${CAP} -IH1
	raidcom add ldev -parity_grp_id 1-2 -ldev_id ${i} -capacity ${CAP} -IH2
done

CAP=”539420672”
raidcom add ldev -parity_grp_id 1-1 -ldev_id 0x3080 -capacity ${CAP} -IH0
raidcom add ldev -parity_grp_id 1-1 -ldev_id 0x3080 -capacity ${CAP} -IH1
raidcom add ldev -parity_grp_id 1-1 -ldev_id 0x3080 -capacity ${CAP} -IH2
raidcom add ldev -parity_grp_id 1-2 -ldev_id 0x3081 -capacity ${CAP} -IH0
raidcom add ldev -parity_grp_id 1-2 -ldev_id 0x3081 -capacity ${CAP} -IH1
raidcom add ldev -parity_grp_id 1-2 -ldev_id 0x3081 -capacity ${CAP} -IH2

for i in $(seq 12288 12417); do
	raidcom initialize ldev -operation qfmt -ldev_id ${i} -IH0
	raidcom initialize ldev -operation qfmt -ldev_id ${i} -IH1
	raidcom initialize ldev -operation qfmt -ldev_id ${i} -IH2
done
```


#### 7. CREATE POOL
---
```bash
for i in $(seq 12288 12417); do
	raidcom add dp_pool -pool_id 0 -pool_name nvmepool -ldev_id ${i} -IH0
	raidcom add dp_pool -pool_id 0 -pool_name nvmepool -ldev_id ${i} -IH1
	raidcom add dp_pool -pool_id 0 -pool_name nvmepool -ldev_id ${i} -IH2
done
```
