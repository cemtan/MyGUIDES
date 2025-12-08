#### ISL
---
---



**1. Decide ISL ports (Use at least two blades and try to use trunk ports)**

**2. Set ports to their defaults**
	portcfgdefault <port>
	
**3. Set port speeds if it is needed**
	portcfgspeed <port> <speed>
	
**4. [OPTIONAL] Apply folowing steps if it is needed**
	1. Enable trunking
		portcfgtrunkport <port> 1 
	2. Disable trunking
		portcfgtrunkport <port> 0
	3. Enable QoS
		portcfgqos --enable <port>
	4. Set distance metrics
		i. For distance less than 10km
			portcfglongdistance <port> LE 1 
		ii. For distance more than 10km (DISTANCE 30 for 30km as an example)
			portcfglongdistance <port> LS 1 -distance <DISTANCE> 
		iii. Verify the distance
			portbuffershow 
	5. Set the general switch settings to default
		configdefault -all 
	6. Clear the Administrative Domain (AD) configuration
		FC_switch_A_1:admin> switch:admin> ad --select AD0
		FC_switch_A_1:> defzone --noaccess
		FC_switch_A_1:> cfgsave
		FC_switch_A_1:> exit
		FC_switch_A_1:admin> ad --clear -f
		FC_switch_A_1:admin> ad --apply
		FC_switch_A_1:admin> ad --save
		FC_switch_A_1:admin> exit
		
**5. Clear zone config**
	cfgdisable
	cfgclear
	defzone --allaccess
	cfgsave
	defzone --show
	
**6. Change Domain ID**
	switchdisable
	configure (reboot is needed)
	switchenable (not needed after reboot)

**7. Disable all ISL ports**
	portdisable -i <port>
	
**8. Connect ISL cables**

**9. Enable all ISL ports**
	portenable -i <port>

**10. Set Fabric Name across all switches**
	fabricname --set
	
**11. Check config and ISL**
	switchshow
	fabricshow
	islshow
	errshow
	porterrshow
	sfpshow <port>
