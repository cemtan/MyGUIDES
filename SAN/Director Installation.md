#### DIRECTOR INSTALLATION
---
---



#### 1. HARDWARE INSTALLATION
---
1. Assembly & Power Up		:	 Hitachi
2. Cabling						        :	Customer
3. Setting login credentials 	    :	Customer
4. Configuring network		    : 	Hitachi


#### 2. FIRMWARE UPGRADE
---
##### 1. Verification
	version 
	firmwareshow 

##### 2. Upgrade FOS from FTP server 
	firmwareDownload 
	firmwareDownloadStatus 


#### 3. CONFIGURATION
---
##### 1. Configuring network
	ifmodeset 
	ipaddrset 
	dnsconfig --add –domain  
	
##### 2. Resetting switch configuration (optional) 
	configdefault -all 
	
##### 3. Enabling Virtual Fabrics (will be discussed) 
	fosconfig --enable vf (reboot is needed) 
	
##### 4. Setting names 
	chassisname <name of chassis> 
	switchName <name of switch> 
	fabricname --clear 

##### 5. Validating / adding all licenses 
	license --show
	license --install -key <license key> 
	
##### 6. Clear default zone config 
	defzone --allaccess 
	
##### 7. Time configuration 
	tsclockserver <ip address>
		> ts.clochassisname ckServer:10.202.110.123
		> ts.clockServerIndex:-1
		> ts.clockServerIndexList:-1;-1
		> ts.clockServerList:10.202.110.123 
	tstimezone Europe/Istanbul
	tstimezone –interactive 
	
##### 8. Syslog definition (optional) 
	syslogadmin --set ip <ip address>
		> syslog.address.1:10.200.142.163 
		
##### 9. Enabling auditing 
	auditcfg --enable -all
	auditcfg -–class 1,2,3,4,5,7,8,9
		> audit.cfg.class:1,2,3,4,5,7,8,9,10,
		> audit.cfg.severity:4
		> audit.cfg.state:1 
		
##### 10. SNMP v3 configuration 
	snmpconfig --set snmpv3
		> snmp.accessList.0.address:10.200.142.0
		> snmp.accessList.1.address:10.202.142.0
		> snmp.snmpv3TrapTarget.0.trapTargetAddr:10.202.142.11
		> snmp.snmpv3TrapTarget.1.trapTargetAddr:10.200.142.163
		> snmp.snmpv3TrapTarget.2.trapTargetAddr:10.202.142.180
		> snmp.snmpv3TrapTarget.3.trapTargetAddr:0.0.0.0
		> snmp.snmpv3TrapTarget.4.trapTargetAddr:0.0.0.0
		> snmp.snmpv3TrapTarget.5.trapTargetAddr:0.0.0.0
		> snmp.sysContact:Field Support.
		> snmp.sysContact.default:Field Support.
		> snmp.sysDescription:Connectrix Fibre Channel Switch.
		> snmp.sysDescription.default:Fibre Channel Switch.
		> snmp.sysLocation:End User Premise.
		> snmp.sysLocation.default:End User Premise.
		> snmp.sysObjectID:1588.2.1.1.1
		> snmp.sysObjectID.default:1588.2.1.1.1
		> snmp.trapEnterpriseFlag:1
		> snmp.trapFilter:1,3;
		> snmp.usmUserPaswdEncFlag:0
		> snmp.v1Enable:1
		> snmp.v1Enable.default:1 
		
##### 11. Creating users 
	 BNA:
		userconfig --add bna_admin -r admin -l 1-128 -h 128 -c admin -d "Admin user for BNA" 
	 SNMP:
		userconfig --add snmpadmin1 -r admin -l 1-128 -h 128 -c admin -d "SNMP user" 
		
##### 12. Setting Relay Server 
	relayconfig --config -rla_ip 10.200.110.210 -rla_dname KKBDOMAIN
	relayconfig --show 
	
##### 13. Enabling MAPS (optional) 
	mapsconfig –enablemaps –policy ?_moderate_policy
	mapsconfig --actions –email
	mapsconfig --emailcfg –address <Group Email Address>
	mapsconfig --emailcfg -from <Switch Email Address>
	mapsconfig --show
	mapsconfig --testmail -subject TEST -message test 
	
##### 14. Security Settings 
###### 1. Disable Telnet ipfilter --clone SecProtoOnly -from default_ipv4
	https://docs.broadcom.com/doc/FOS-90x-Admin-AG  
		ipfilter --delrule SecProtoOnly -rule 2
		ipfilter --addrule SecProtoOnly -rule 2 -sip any -dp 23 -proto tcp -act deny
		ipfilter --activate SecProtoOnly
		ipfilter --save SecProtoOnly

###### 2. Disable HTTP ipfilter --clone SecProtoOnly -from default_ipv4
	https://docs.broadcom.com/doc/FOS-90x-Admin-AG  
		ipfilter --delrule SecProtoOnly –rule 3
		ipfilter --addrule SecProtoOnly -rule 3 -sip any -dp 80 -proto tcp -act deny
		ipfilter --activate SecProtoOnly
		ipfilter --save SecProtoOnly

###### 3. Disable SNMP v1
	https://docs.broadcom.com/doc/FOS-90x-Admin-AG  
		snmpconfig –-disable snmpv1
		snmpconfig --show snmpv1
			
##### 15. Setting Domain ID 
	switchdisable 
	configure 
	switchenable 
	switchshow 
	
##### 16. Defining routing policies 
	aptpolicy 3 (default) 
	iodreset (iodshow) 
	dlsset –-enable -lossless(dlsshow) 
	
##### 17. Disabling bottleneck monitoring 
	bottleneckmon–disable 
	
##### 18. Defining switch status policy 
	switchstatuspolicyset 
	switchstatuspolicyshow 
	
##### 19. Switch status verification 
	switchshow 
	secpolicyshow 
	portcfgshow 
	switchstatusshow 
	fanshow 
	sensorshow 
	tempshow 
	chassisshow 
	slotshow 
	portshow 
	errshow 
	hashow 
	
##### 20. Clearing all errors 
	errclear 
	slotstatsclear 
	statsclear 
	
##### 21. Saving configuration (optional) 
	configupload –all 
	supportsave 

##### 22. Additional Commands
	snmpconfig --show accessControl
	snpconfig --set seclevel
	snpconfig --default snmpv3



#### 4. BNA or SANNAV Integration
---


#### 5. LINKS
---
Brocade® X7-4 Director Hardware Installation Guide
- CR64-4 Port Numbering
https://techdocs.broadcom.com/us/en/fibre-channel-networking/directors/x7-4-director/1-0/v25833929/v25838698.html

- ICL Trunking Groups
https://techdocs.broadcom.com/us/en/fibre-channel-networking/directors/x7-4-director/1-0/v25833929/v25838780.html

- ICL Cabling Configurations
https://techdocs.broadcom.com/us/en/fibre-channel-networking/directors/x7-4-director/1-0/v25833929/v25838805.html

- FC32-X7-48 Port Blade Numbering and Trunking
https://techdocs.broadcom.com/us/en/fibre-channel-networking/directors/x7-4-director/1-0/v25888256/v25839448/v25839493.html

Brocade® Fabric OS® Administration Guide, 9.2.x
- Port Groups for Trunking (G620)
https://techdocs.broadcom.com/us/en/fibre-channel-networking/fabric-os/fabric-os-administration/9-2-x/v26799888/v26800099.html

- Director Port Blades
https://techdocs.broadcom.com/us/en/fibre-channel-networking/fabric-os/fabric-os-administration/9-2-x/Perform-Advance-Configuration-Tasks-Admin1/v26747397/v26748697.html

Brocade® Fabric OS® Command Reference Manual, 9.2.x
https://techdocs.broadcom.com/us/en/fibre-channel-networking/fabric-os/fabric-os-commands/9-2-x/Fabric-OS-Commands/portCfgPersistentDisable.html