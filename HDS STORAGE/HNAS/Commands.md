#### COMMANDS
---
---



##### OWNER PROBLEM FIX
---

	evs list
	evs-select 3
	filesystem-list --all
	selectfs new_witness

	→  Set Owner as BUILTIN users
		cacls-set owner S-1-5-32-544 winpavopaycls
		cacls-add dacl ace 4 flags 0x10 fullcontrol who S-1-5-32-544 winpavopaycls
		cacls-add dacl ace 4 flags 0x1b fullcontrol who S-1-5-32-544 winpavopaycls
		cacls-add dacl ace 4 flags 0x12 mask 0x100002 who S-1-5-32-545 winpavopaycls
		cacls-add dacl ace 4 flags 0x12 mask 0x100004 who S-1-5-32-545 winpavopaycls
		cacls-add dacl ace 4 flags 0x13 mask 0x1200a9 who S-1-5-32-545 winpavopaycls



##### PRIVATE FOLDERS
---

	umask-directory-set 0077
	umask-file-set 0077



##### SMB CLIENT DENIAL https://knowledge.hitachivantara.com/portal/app/portlets/results/viewsolution.jsp?solutionid=240403060156251&guest=0
---

	smb-barred-client-add
	smb-barred-client-remove
	smb-barred-client-clear
	smb-barred-client-list

Here are the two commands that are used to configure these thresholds:

	set-smb-auto-barring-mean-interval-threshold-in-seconds

This is default to 2 seconds and is configurable with minimum 1s and maximum 120s.

	set-smb-auto-barring-sample-size

This is default to 21 samples and is configurable with minimum 1 and maximum 512.



##### OTHERS
---

###### VLAN Create
https://knowledge.hitachivantara.com/Documents/Storage/NAS_Platform/13.8/NAS_Administration_Guides/Network_Administration_Guide/20_Adding_VLAN_interfaces

###### Filesystem-expand
	filesystem-expand --storage-is-lightly-loaded --to 1200 Marketing

###### Command Set 1
	nfs-max-supported-version 4.1
	cifs-min-supported-version
	cifs-share
	cifs-share list <SHARE>
	cifs-saa
	cifs-auth list
	cifs-name list -a
	fsm connection -v all
	fsm security-decode-file <pathname> (e.g. fsm security-decode-file /myfilesystem/myexportdirectory/mydirectory/myfile ; fsm security-decode-file /myfilesystem/myexportdirectory/mydirectory).
	user-mappings-list
	group-mappings-list
	cifs-dc-errors
	cifs-dc-stats
	cifs-errors
	cifs-stats
	dblog last
	diagshowall
	directory-services-config-dump
	domain-mappings-list
	event-log-show
	filesystem-list --all
	fs-error-log -f <FS>
	smb-client-stats
	smb-client-errors
	smb-client-connection-list --show-totals -v
	smb2-session-connection-list --all
	mgmnt-log-show
	cifs-restrict-anonymous disable
	max-dir-size --total-chunks 0x3000000
	shot-stats running
	cifs-smb2-signing-requried-default
	cifs-smb3-signing-required-default
	set replication-ping-wait-s 60
	snapshot-list --file-system MarketingFS
	snapshot-delete --file-system MarketingFS AUTO_SNAPSHOT_61597464-341b-11da-915f-040700010900_9311
	
	selectfs <fsname>
	cacls-add dacl ace 4 flags 0x3 fullcontrol who S-1-5-21-868808838-1221903022-1736980299-513 .
	credentials-reassess
	security-decode-file .
	cacls-del dacl ace 5 .
	
	rpc-service-nfs-udp --disable

###### Command Set 2
	evs-select <ID>
	evs-security list | individual -e <evs-id>  | global -e <evs-id>
	routing-by-evs-disable     Supervisor           Disable routing by EVS
	routing-by-evs-enable     Supervisor           Enable routing by EVS
	routing-by-evs-show        Supervisor           Show state of routing by EVS
	test-route            Supervisor           tests the router
	test-route-by-evs              Supervisor           tests the router
	route     User       Display/Configure Route Table
	route-flush          Supervisor           Flush the router cache
	route-gateway-add           Supervisor           Add a gateway route
	route-gateway-delete      Supervisor           Delete a gateway route
	route-host-add   Supervisor           Add a host route
	route-host-delete             Supervisor           Delete a host route
	route-list              User       Display standard route registry configuration
	cifs-max-supported-version [ --default | 1 | 2 | 2.1 | 3 ]
	cifs-config-clear Supervisor           Clear all CIFS serving names for the EVS in context.
	cifs-config-list     Supervisor           List currently-configured CIFS serving names for the EVS in context.
	cifs-dc   Supervisor           Manages the Domain Controller cache.
	cifs-dc-broadcast              Supervisor           Used to enable/disable domain controller discovery using NetBIOS broadcast
	cifs-dc-clock-accept         Supervisor           Adjust tolerance of server / DC clock skew.
	cifs-dc-discovery-stats     Supervisor           Report Domain Controller discovery statistics
	cifs-dc-errors      Supervisor           Report CIFS DC protocol errors
	cifs-dc-stats        User       Report Domain Controller statistics
	cifs-name

###### Ports & Fabric
	fc-fabric (view fabric)
	fc-hports (view host ports)
	fc-link (enabled/disable)
	fc-ports (view all remote target ports)
	fc-nodes (view WW name of target nodes)

###### Examples
	List all configured CIFS serving names for all EVSs:
	cifs-name list -a
	
	List all online CIFS serving names for EVS 3:
	vn 3 cifs-name list -o
	
	Add an NT4-mode CIFS serving name to the current EVS:
	cifs-name add -m nt4 -d my-domain my-nt4-name
	
	Add an ADS-mode CIFS serving name to the current EVS (prompts are issued for the authorized username and password values):
	cifs-name add -m ads -a 10.2.1.123 my-ads-name
	cifs-name add -m ads -a 10.2.1.123 -f "ou=midorg, ou=toporg" ads-name2
	cifs-name add -m ads -a 10.2.1.123 -f midorg.toporg my-ads-name3
	
	Using IPv6:
	cifs-name add -m ads -a fdca:f995:220a:480:e89b:de48:795f:8a01 my-ads-name
	
	Remove a CIFS serving name from the current EVS:
	cifs-name remove my-cifs-name
	
	Remove all CIFS serving names from the current EVS:
	cifs-name remove -a

###### CIFS 
	cifs-share
	cifs-state
	cifs-auth [on|off|list]
	dnsdomainname		User       Define DNS domain name
	dnssearch            	User       Define DNS search list
	dnsserver             	User       Define DNS servers

	Add Permission
	cifs-saa add SHARE2 DOMAIN\User af → give full control
	cacls-set owner <sid value> <file name> → change owner


###### NTP Operation
	ntp operation
	where 'operation' is one of:
				   list
				   add server1 [server2] ... [server5]
				   del server1 [server2] ... [server5]

###### EVS
	evs
	   create [-l <label>] -i [<ipaddr/prefix> | <ipaddr> -m <mask>] -p <port> [-n <dst-nodeid>][-w <witness-for>]
	   delete -e <evs-specifier> [-c | --confirm]
	   enable -e <evs-specifier> [-n <dst-nodeid>]
	   disable -e <evs-specifier> [-c | --confirm]
	   restart -e <evs-specifier> [-c | --confirm]
	   migrate ( -e <evs-specifier> | --all <src-nodeid> ) -n <dst-nodeid> [-c | --confirm]
	   relabel -e <evs-specifier> -l <new-evs-label>
	   list [-t <admin | serv | witness> | -e <evs-specifier>]

###### ADS
	If you adding ADS domain, complete the following steps:
		a. Select the ADS option.
		b. In the IP Address field, specify the IP address of a domain controller in the Active Directory.
		c. In the DC Admin User field, specify a user account that is a member of the Domain Administrators group. This privilege is necessary to create a computer account in the Active Directory. When specifying a user account from a trusted domain, the user account must be entered using the Kerberos format; that is, administrator@ADdomain .mycompany.com, not ADdomain\administrator.
		d. In the DC Admin Password field, specify the password associated with the DC admin user name specified.
		e. In the Folder field, specify the name of the folder in the Active Directory in which the computer account should be created. By default, the computer account will be created in the Computers folder.

###### NFS Uuser Mappings https://knowledge.hitachivantara.com/?title=Knowledge%2FStorage%2FNetwork_Attached_Storage%2FHNAS_NFSv4_Export_Shows_File_Ownership_as_%2522Nobody%2522
**Home** → **File Services** → **User Mappings**
→ user-mappings-list
→ group-mappings-list
→ user-mappings-modify --> UnixName@NFSv4Domain
→ group-mappings-modify

	selectfs <fs x>
	->cd to root of folder location
	security-decode-file <folder containing file>
	ls -la 
	->cd into folder
	security-decode-file <file>
	ls -la 
	credentials-list
	fs-dacl-mode
	fs-dacl-mode mask-on-chmod-discard-on-create (?) (clusterwide)  -----> FOR NFS 4.1

###### NFS v4
NFSv4 Inheritance Not Working From The UNIX Side
https://knowledge.hitachivantara.com/Knowledge/Storage/Network_Attached_Storage/Hitachi_NAS_Platform/NFSv4_Inheritance_Not_Working_From_The_UNIX_Side

	vn 1
	it should be mixed: 
		security-mode set -f  <FS> mixed
	if it is unix:
		selectfs <FS>
		fs-dacl-mode mask-on-chmod-or-create

###### AD Troubleshooting
	diagshowall
	trouble
	trouble ldap
	vn all cifs-dc list -v
	ldap-search-config
	nolog pn 1 lt du paced-event-logger
	vn all group-mappings-list
	cifs-keytab-list --all
	
	vn 1 traceroute-from-evs -i <Local IP Address> <Server IP Address>
	vn 2 traceroute-from-evs -i <Local IP Address> <Server IP Address>

###### LDAP
	ldap-search-config 
		ldap-search-config --search-scope | -s  one-level | sub-tree
		ldap-search-config --common | -c    <common search base DN>
		ldap-search-config --user | -u      <user search base DNs>
		ldap-search-config --group | -g     <group search base DNs>
		ldap-search-config --netgroup | -n  <netgroup search base DNs>
		ldap-search-config --host | -h      <host search base DNs>
		
###### Heiko
	cifs-spnego-auth -l ntlmssp
	cluster-show -qy
	pn all agg
	vn all dnsserver list
	evs-security list
	pn all ifconfig ag1
	vn 0
	route
	evs list
	cluster-icc-show
	evs migrate -e 0 -n 2 -c
	evs migrate -e 1 -n 2 -c
	ver
	pn all agg
	vn all cifs-dc list -v
	nolog lt du paced-event-logger
	smb-client-errors
	smb-client-connection-list
	cifs-errors
	cifs-dc get
	cifs-dc list
	set netlogon-rpc-aes true
	set | grep netlogon-rpc-aes
	lt du paced-event-logger
	pn all fsi-cache-flush
	vn all credentials-reassess
	cifs-kdc-comms
	cifs-config-list -p
	Start-Process -FilePath cmd.exe /c -Credential (Get-Credential -UserName CITRIX-EVS1$ -Message 'Test Credential')
	Get-ADComputer -Identity "CITRIX-EVS1" -Properties * > CITRIX-EVS1.txt

###### SAMR
https://nam04.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fprevious-versions%2Fwindows%2Fit-pro%2Fwindows-10%2Fsecurity%2Fthreat-protection%2Fsecurity-policy-settings%2Fnetwork-access-restrict-clients-allowed-to-make-remote-sam-calls&data=05%7C02%7Ccem.tan%40hitachivantara.com%7C8c817a5f858e4fa550c008dd4cce0d97%7C18791e1761594f52a8d4de814ca8284a%7C0%7C0%7C638751170089663116%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=PWnWzGjiF45ffZIJNTTGoJmgUYQZvThhlkmAv8Ykjiw%3D&reserved=0

	







