### 3DC RECOVERY 📝
---
---



#### 1. IO MODES
---

![st_3dc13.png](../../../.images/st_3dc13.png)



#### 2. PAIR STATES
---

![st_3dc14.png](../../../.images/st_3dc14.png)



#### 3. GAD IO FLOW
---

##### GAD Write I/O
When GAD status is mirrored the I/O mode of the pairs is mirror (RL)
		Write requests are written to both pair volumes and then a write completed response is returned to the host
		![st_3dc15.png](../../../.images/st_3dc15.png)

##### GAD Read I/O
When GAD status is mirrored the I/O mode of the pairs is mirror (RL)
		Read requests are read from the volume connected to the server and then sent to the server
		There is no communication between the primary and secondary storage system
		![st_3dc16.png](../../../.images/st_3dc16.png)

##### Failure
If a failure occurs preventing host access to a volume in a GAD pair
			Multipathing software will recognize the failure
			If connectivity is available I/O can continue on the other storage system
			![st_3dc17.png](../../../.images/st_3dc17.png)

