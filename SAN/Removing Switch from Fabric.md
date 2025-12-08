#### Removing Switch from Fabric
---
---



**1. Disable ISL ports**
	
	portcfgpersistentdisable <ports>
	
**2. Disconnect ISL cables**

**3. Disable switch and clear configuration**

	configdefault -all 
	switchcfgpersistentdisable
	cfgdisable
	cfgclear
	cfgshow
	defzone --noaccess
	cfgsave
