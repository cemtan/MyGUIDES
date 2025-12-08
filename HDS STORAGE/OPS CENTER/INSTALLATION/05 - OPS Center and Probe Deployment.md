#### OPS CENTER & PROBE DEPLOYMENT
---
---



##### 1. Deploy OpsCenterVM_version.ova
---

**1. Login from console and change password.**
	**USER**: root
	#password **PASSWORD**: manager
	
**2. Run opsvmsetup and change following settings:**
		Host name (or FQDN)
		IP address
		Default gateway
		Network mask
		DNS server (up to two servers)
		Time zone
		NTP server
	Shutdown and reconnect network adapter.
	
**3. Power on**

**4. Create /HDID-METADATA folder for Protector metadata.**
	Make mode 755 for metadata folder.

**5. Login from UI**
	https://host-name-or-IP-address-of-Portal:port-number/portal/
	**USER**: sysadmin
	#password **PASSWORD**: sysadmin

**6. Generate and install a signed SSL certificate. By default, the Ops Center Administrator installation package comes with a self-signed certificate that you can use for the initial log in to Ops Center Administrator.**



##### 2. Deploy dcaprobe_version.ova
---

**1. Login from console and change password.**
	**USER**: root
	#password **PASSWORD**: manager
	
**2. Run opsvmsetup and change following settings:**
	**Network**
		**Host name**: The Analyzer probe server does not support FQDNs. Omit the domain name when specifying the host name.
		**DHCP**: RAID Agent does not support the use of DHCP. If you are using RAID Agent, specify n.
		IP address
		Default gateway
		Network mask
		DNS server (up to two servers)
		Time Settings
	**Time zone**: {area/location}
		Use timedatectl list-timezones command to list time zones.
			Analyzer server & Analyzer detail view server must be synced.
		NTP Server
	**Security Setting**
		Server Certificate
	**Protector Settings**
		Whether to use Protector  yes
		Protector master host name  hostname
		Protector master IPv4 address  IP address of OPS center
	 After the settings are applied, the guest OS restarts automatically.
	 Shutdown and reconnect network adapter.
	 
**3. Power on**

**4. Create /HDID-METADATA folder for Protector metadata.**
	Make mode 755 for metadata folder.
