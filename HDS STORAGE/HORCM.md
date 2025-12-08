#### HORCM File
---


```bash
HORCM_MON
#ip_address	service	poll(10ms)		timeout(10ms)
localhost		31000	1000			3000

HORCM_CMD
# dev_name
#\\.\IPCMD-<IP1>-31001 \\.\IPCMD-<IP2>-31001
/dev/<BLOCK DEVICE>

HORCM_LDEV
#GRP				DEV					SERIAL	LDEV#	MU#

HORCM_INST
#GPR				IP ADR	PORT#
```


#### HORCM with Container
---

##### Podman:
	podman exec -it cli_serial_of_array bash
##### Docker:
	docker exec -it cli_serial_of_array bash
    
###### Example:
	podman exec -it cli_60019 bash
	raidcom get ldev -ldev_id X -I7 (where X=one of the affected devices)
