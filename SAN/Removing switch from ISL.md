#### Removing switch from ISL
---
---



Removing a switch from the fabric is a disruptive operation. You must execute the chassisdisable command, which disables all logical switches in VF mode or disable the switch using the switchdisable command in non-VF mode.

	fabricprincipal --enable -p 0x01 (on new switch)
	# chassisdisable
	switchCfgPersistentDisable
	sysshutdown
