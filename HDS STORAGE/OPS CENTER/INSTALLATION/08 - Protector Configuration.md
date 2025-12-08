#### CHANGING PROTECTOR MASTER
---
---


```bash
setconfig -c
setconfig -m <masterip>
```
Following Ports should be open between probe master and all clients
	
	30304 TCP
	20964 TCP
	38102 TCP


**Hitachi Ops Center Protector User Guide**

1. Open a command prompt as administrator.
2. Change directory to the <Ops Center Protector install>\bin folder.
3. Stop the Protector services by entering the command diagdata --stop hub
4. Check the node’s current configuration by entering the command setconfig –l
	Refer to "Changing a node's profile with setconfig" in User Guide.
5. To configure a preferred IP address for the node, enter the command setconfig –p nnn.nnn.nnn.nnn where nnn.nnn.nnn.nnn is the IP address for the node on the preferred network.
	Additional addresses may be added by listing them in order of preference separated by semicolons. Multiple addresses may need to be quoted depending on Operating System.
6. To configure a barred IP address for the node, enter the command setconfig –b nnn.nnn.nnn.nnn where nnn.nnn.nnn.nnn is the IP address for the node on the barred network.
	Additional addresses may be added by listing them in order of preference separated by semicolons. Multiple addresses may need to be quoted depending on Operating System.
7. To configure an external (internet) IP address for the node, enter the command setconfig –x nnn.nnn.nnn.nnn where nnn.nnn.nnn.nnn is the IP address for the node on the internet.
	Additional addresses may be added by listing them in order of preference separated by semicolons. Multiple addresses may need to be quoted depending on Operating System.
8. If you need to remove an address then enter the command setconfig –r nnn.nnn.nnn.nnn where nnn.nnn.nnn.nnn is the IP address.
	Additional addresses may be added by listing them in order of preference separated by semicolons. Multiple addresses may need to be quoted depending on Operating System.
	Note: The address(es) are removed from the preferred, barred and fixed list.
9. Review the node’s new configuration by re-entering the command setconfig –l
10. Restart the Protector services by entering the command diagdata --start hub
11. Wait a few minutes, then reauthorise the node on the Master (See "How to authorize a node" in User Guide).
