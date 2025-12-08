### DELETING HUR
---
---



#### 1. Delete HUR disk's snapshots
---

#### 2. Delete HUR
---

##### A. PROTECTOR
1. **Storage** → **3dc Teardown**
2. if policy used by others
			**Dataflow** → **Delete** 
			**Policy** → **New GAD Policy**
			**Dataflow** → **Adopt existing gad pairs** (Mirrorid will be asked → pairdisplay, navigator → remote rep / gad)
	  else
			**Policy** → **Edit**
			**Dataflow** → **Delete HUR and Delta**
     Delete HUR ldev if not deleted already


##### B. CLI
1. **HUR split from primary** → pairsplit -S
2. Unmap HUR LDEVs
3. Delete HUR ldevs


##### C. NAVIGATOR
1. **Remote Replication** → **HUR from primary**
2. Select LDEV
3. **Menu** → **Delete Pair** (do not change to read / write)
4. Unmap HUR LDEVs
5. Delete HUR LDEVs
