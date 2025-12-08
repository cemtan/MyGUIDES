#### UPGRADE - LONG LIST
---
---



##### 1. Common Services
---

	[root@hdsopstml COMSERV]# ls
	cs  csinst.properties  cssetup.tar  install.sh  rpm
	[root@hdsopstml COMSERV]# ./install.sh
	Starting Hitachi Ops Center Common Service installation.
	The following product will be installed on this system:
	  Hitachi Ops Center Common Service 10.8.3-00 [Upgrade]
	
	Caution: Quit all programs. If security, process, and virus monitoring programs are running, the installation can fail.
	
	Caution: Common Services will install the following rpm packages/versions. If other products on the server use these packages, confirm whether the products support the version listed.
	  java-11-amazon-corretto-devel-11.0.15.9-1.x86_64.rpm
	  postgresql11-11.16-1PGDG.rhel7.x86_64.rpm
	  postgresql11-libs-11.16-1PGDG.rhel7.x86_6	4.rpm
	  postgresql11-server-11.16-1PGDG.rhel7.x86_64.rpm
	
	Press Enter to continue.
	
	<<Current Settings>>
	
	Installation destination directory:
	  /opt/hitachi/CommonService
	The storage destination for user data files:
	  /var/opt/hitachi/CommonService
	The storage destination for log files(Unmodifiable):
	  /var/log/hitachi/CommonService
	
	Back up the database before installing: Yes
	  The backup database destination:
		/var/opt/hitachi/CommonService/backup
	
	To leave things as they are and not make changes, press y.
	To make changes, press e.
	To cancel installation, press n.
	> y
	
	Stopping CommonService : CommonService is running. These services will be stopped during installation if you choose to continue. you want to continue installation? (y/n)
	>y
	  Now performing pre-installation processing...
	  Now installing Common Service...
	  Now setting up database...
	  Now setting up gateway ...
	  Now setting up Common Service...
	  Now performing post-installation processing...
	  Now starting the Common Service...
	Hitachi Ops Center Common Service installation completed successfully.


##### 2. Administrator
---
	
	[root@hdsopstml dir]# ./install.sh
	[Info] Making log directory for installation.
	[Info] Made log directory for installation.
	[Info] Docker is installed as container runtime.
	Enter username for the service account credentials: sysadmin
	Enter sysadmin's Password:
	Older version exists. Do you want to upgrade? [y/n]: y
	Do you wish to configure Ops Center Common Services [y/n]: y
	Enter URI to Ops Center Common Services portal: https://hdsopstml.turkcell.entp.tgc/portal
	Provide signing certificate for Ops Center Common Services. Press [Enter] if you do not have one []:
	Enter Ops Center Common Services admin account username: sysadmin
	Enter Ops Center Common Services admin account password:
	Enter Ops Center Administrator application name [Administrator]:
	Enter Ops Center Administrator application description value []:
	[Info] Found old virtual appliance manager container 1bcc6e1cafb2.
	[Info] Found old APP_MANAGER_CONTAINER_DOMAIN value: hid.local.
	[Info] Found old APP_MANAGER_INSTANCE_ID value: 8bf4adc3-0778-4b02-9bf2-33483c3156d2.
	[Info] Found old APP_MANAGER_OVF_MODE value: false.
	[Info] Getting host IP address.
	[Info] Successfully got host IP Address: {"hostIp":"10.207.130.110","snmpIp":"10.207.130.110","httpsPort":"20961"}
	[Info] Getting application logs.
	[Info] Successfully got application logs: rainier-logs-20220920-220612.tar.gz
	[Info] Getting audit logs.
	[Info] Successfully got audit logs: rainier-audit-logs-20220920-220632.tar.gz
	[Info] Getting backup data.
	[Info] Successfully got backup data: /tmp/rainier-backup-20220920-220632.tar.gz
	[Info] Stopping older virtual appliance manager container.
	[Info] Successfully stopped older virtual appliance manager container.
	[Info] Making log directory for tools.
	[Info] Made log directory for tools.
	[Info] Installing rainier-getlogs command.
	[Info] Installed rainier-getlogs command.
	[Info] Installing rainier-replace-jdk command.
	[Info] Installed rainier-replace-jdk command.
	[Info] Installing rainier-memory-setting command.
	[Info] Installed rainier-memory-setting command.
	[Info] Installing setupcommonservice command.
	[Info] Installed setupcommonservice command.
	[Info] Loading images.
	[Info] Loaded images.
	[Info] Setting DNS bind folder to /var/lib/docker/volumes/84963f1c48b3551a56f014d1af3b3ede793619cf6a87659016382b4086bc5c97/_data.
	[Info] Creating container app-manager-wolf.28523.
	[Info] Completed creating container app-manager-wolf.28523.
	[Info] Waiting for the virtual appliance manager is ready.
	........................................................................................................................
	[Info] Virtual appliance manager is ready.
	[Info] Restoring host IP address.
	[Info] Successfully restored host IP Address: {"hostIp":"10.207.130.110","snmpIp":"10.207.130.110","httpsPort":"20961"}
	[Info] Adding applications.
	[Info] Adding application: /root/ops_center_update/dir/applications/infra.tar.gz
	[Info] Successfully added application: /root/ops_center_update/dir/applications/infra.tar.gz.
	[Info] Adding application: /root/ops_center_update/dir/applications/rainier.tar.gz
	[Info] Successfully added application: /root/ops_center_update/dir/applications/rainier.tar.gz.
	[Info] Waiting for the service to start.
	......................................
	[Info] Waited 41 seconds
	[Info] The service is ready.
	[Info] Restoring backup data: /tmp/rainier-backup-20220920-220632.tar.gz
	[Info] Successfully restored backup data.
	[Info] Removing older virtual appliance manager container.
	[Info] Successfully removed older virtual appliance manager container.
	[Info] Removing older container images.
	[Info] Successfully removed older container images.
	Checking for application name Administrator now...
	Application has been registered. Deleting...
	Registering a new application
	Application Registration Successful
	Common Services setup succeeded
	Done! Took 20 minutes and 36 seconds.
	

##### 3. Configuration Manager
---

###### OPS
	
	[root@hdsopstml Linux]# ./install.sh
	Starting Configuration Manager REST API installation.
	The following product will be installed on this system:
	  Configuration Manager REST API 10.8.3-00 [Upgrade]
	
	Caution: Quit all programs. If security, process, and virus monitoring programs are running, the installation can fail.
	
	Press Enter to continue.
		
	<<Current Settings>>
	
	Installation destination directory:
	  /opt/hitachi
	
	Back up the database before installing: Yes
	  The backup database destination:
		/opt/hitachi/backup
	
	To leave things as they are and not make changes, press y.
	To make changes, press e.
	To cancel installation, press n.
	> y
	WARNING: The following directory is going to be removed because the backup directory already exists.
		/opt/hitachi/backup/bak_CONFIG_MGR
	Continue? (y/n)
	> y
	
	Progress
	  Now stopping the Configuration Manager REST API server services...
	  Now backing up the database...
	  Now performing pre-installation processing...
	  Now installing Configuration Manager REST API...
	  Now performing post-installation processing...
	  Now starting the Configuration Manager REST API server services...
	Configuration Manager REST API installation completed successfully.
	

###### PROBE

	[root@HDSPROBETML Linux]# ./install.sh
	Starting Configuration Manager REST API installation.
	The following product will be installed on this system:
	  Configuration Manager REST API 10.8.3-00 [Upgrade]
	
	Caution: Quit all programs. If security, process, and virus monitoring programs are running, the installation can fail.
	
	Press Enter to continue.
	
	
	<<Current Settings>>
	
	Installation destination directory:
	  /opt/hitachi
	
	Back up the database before installing: Yes
	  The backup database destination:
		/opt/hitachi/backup
	
	To leave things as they are and not make changes, press y.
	To make changes, press e.
	To cancel installation, press n.
	> y
	Progress
	  Now stopping the Configuration Manager REST API server services...
	  Now backing up the database...
	  Now performing pre-installation processing...
	  Now installing Configuration Manager REST API...
	  Now performing post-installation processing...
	  Now starting the Configuration Manager REST API server services...
	Configuration Manager REST API installation completed successfully.
	
	
##### 4. Analyzer
---

###### OPS

	[root@hdsopstml cd]# ls
	ANALYTICS  DCAPROBE  DCAWINPROBE  TOOLS
	[root@hdsopstml cd]# cp -pr ANALYTICS/ ../dir/
	[root@hdsopstml cd]# cd ../dir/ANALYTICS/
	[root@hdsopstml ANALYTICS]# ls
	analytics_install.sh  analytics_precheck.sh  analytics_uninstall.sh  DCA  IAA
	[root@hdsopstml ANALYTICS]# cd ..
	[root@hdsopstml cd]# cp -pr ANALYTICS/ ../dir/
	[root@hdsopstml cd]# cd ../dir
	[root@hdsopstml dir]# cd ANALYTICS/
	[root@hdsopstml ANALYTICS]# ls
	analytics_install.sh  analytics_precheck.sh  analytics_uninstall.sh  DCA  IAA
	[root@hdsopstml ANALYTICS]# sh ./analytics_precheck.sh
	============================================================
	 Analytics Precheck                          ver. 10.8.3-00
	============================================================
	
	[ Check results ]
	Ops Center Analyzer detail view server [10.8.3-00]      [OK]
	Ops Center Analyzer server [10.8.3-00]                  [OK]
	
	[ Details ]
	Check premise OS version.                               [OK]
	
	[ Details related to the Ops Center Analyzer detail view server ]
	Check required RPM packages.                            [OK]
	Check required command path.                            [OK]
	Check recommended command path.                         [OK]
	Check java environment.                                 [OK]
	Check kernel info.                                      [OK]
	Check directory for noexec mount option.                [OK]
	
	[ Details related to the Ops Center Analyzer server ]
	Check disk size for work directory.                     [OK]
	Check required RPM packages.                            [OK]
	Check other products that use the Common component.     [OK]
	Check directory for noexec mount option.                [OK]
	
	[root@hdsopstml ANALYTICS]# sh ./analytics_install.sh VUP
	Do you want to install the Ops Center Analyzer detail view server? (y/n) [n]: y
	
	Do you want to install the Ops Center Analyzer server? (y/n) [n]: y
	
	
	[Confirmation]
	------------------------------------------------------------
	Installation Product
	(1) Ops Center Analyzer detail view server
	(2) Ops Center Analyzer server
	------------------------------------------------------------
	Do you want to install the server listed above? (y/n) [n]: y
	
	[INFO] Analytics installer started
	============================================================
	Installation of the Ops Center Analyzer detail view server
	============================================================
	[INFO] Installation of the Ops Center Analyzer detail view server started.
	[INFO] Stopping crond service.
	[INFO] Crond service stopped.
	[INFO] Stopping the Ops Center Analyzer detail view server service...
	[INFO] The Ops Center Analyzer detail view server service stopped.
	[INFO] Deploying files...
	[INFO] File deployment is complete.
	[INFO] Applying environment settings...
	[INFO] Starting the Ops Center Analyzer detail view server service...
	[INFO] The Ops Center Analyzer detail view server service started.
	[INFO] Waiting for the upgrade process to finish . . .  [0]
	[INFO] Stopping the Ops Center Analyzer detail view server service...
	[INFO] The Ops Center Analyzer detail view server service stopped.
	[INFO] Starting the Ops Center Analyzer detail view server service...
	[INFO] The Ops Center Analyzer detail view server service started.
	[INFO] Starting crond service.
	[INFO] Crond is running.
	[INFO] Environment settings have been applied.
	[INFO] Installation of the Ops Center Analyzer detail view server finished successfully.
	
	
	
	============================================================
	Installation of the Ops Center Analyzer server
	============================================================
	[INFO] Installation of the Ops Center Analyzer server started.
	
	The common component is already installed.
	To install the Ops Center Analyzer server, the common component service must be stopped.
	Do you want to stop the common component service? (y/n) [n]: y
	
	
	
	Do you want to continue the installation? (y/n) [n]: y
	
	[INFO] Stopping the common component service.
	[INFO] The common component service stopped successfully.
	[INFO] Deploying files...
	[INFO] File deployment is complete.
	[INFO] Installation of the common component started.
	[INFO] Installation of the common component finished successfully.
	[INFO] Restarting the common component service.
	[INFO] The common component service restarted successfully.
	[INFO] Environment settings have been applied.
	[INFO] Installation of the Ops Center Analyzer server finished successfully.
	[INFO] Deploying common files...
	[INFO] Common file deployment is complete.
	[INFO] Analytics installer finished
	
	
###### PROBE
	
	[root@HDSPROBETML ~]# cd DCAPROBE
	[root@HDSPROBETML DCAPROBE]# ls
	AGENTS
	agent_update.sh
	bin
	CPAN
	dca_config.exp
	dca_install_device_validator_for_express_installer.sh
	DCA_PROBE
	dcaprobe_install.sh
	dcaprobe_precheck.sh
	dcaprobe_uninstall.sh
	dca_uninst.exp
	HIAA_Config.pm
	iaa_inst_cmd_wrapper.sh
	iaa_inst_common_utility.sh
	iaa_inst_constants.sh
	iaa_inst_eachPP_utility.sh
	iaa_inst_param.conf
	iaa_msg_handler.sh
	iaa_msg_resource.sh
	iaa_PP_utility.sh
	iaa_precheck_utility.sh
	raid_uninst.exp
	REALTIME
	sample
	[root@HDSPROBETML DCAPROBE]# sh ./dcaprobe_precheck.sh
	============================================================
	 Ops Center Analyzer Probe Precheck          ver. 10.8.3-00
	============================================================
	
	[ Check results ]
	Ops Center Analyzer probe server [10.8.3-00]            [OK]
	
	[ Details ]
	Check resolved hostname. [HDSPROBETML (10.207.130.111)] [OK]
	Check premise OS version.                               [OK]
	Check installed Analyzer probe server. [10.8.0-01]      [OK]
	Check required RPM packages.                            [OK]
	Check required command path.                            [OK]
	Check recommended command path.                         [OK]
	Check java environment.                                 [OK]
	Check kernel info.                                      [OK]
	Check directory for noexec mount option.                [OK]
	
	==========================================================================
	Ops Center Analyzer Virtual Storage Software Agent Precheck ver. 10.8.3-00
	==========================================================================
	
	[ Check results ]
	Hitachi Ops Center Analyzer Virtual Storage Software Agent [10.8.3-00][OK]
	
	[ Details ]
	Check prerequisite OS version.                                        [OK]
	Check required RPM packages.                                          [OK]
	
	[root@HDSPROBETML DCAPROBE]# sh ./dcaprobe_install.sh VUP
	[INFO] Installation of the Ops Center Analyzer probe servers started.
	[INFO] Deploying common files...
	[INFO] Common file deployment is complete.
	[INFO] Installation of the On-demand Real Time Monitoring module started.
	[INFO] Installation of the On-demand Real Time Monitoring module finished successfully.
	[INFO] Installation of the RAID Agent started.
	[INFO] Installation of the Ops Center Analyzer probe server started.
	[INFO] Stopping crond service.
	[INFO] Crond service stopped.
	[INFO] Stopping the Ops Center Analyzer probe server service...
	[INFO] The Ops Center Analyzer probe server service stopped.
	[INFO] Deploying files...
	[INFO] File deployment is complete.
	[INFO] Applying environment settings...
	[INFO] Starting the Ops Center Analyzer probe server service...
	[INFO] The Ops Center Analyzer probe server service started.
	[INFO] Waiting for the upgrade process to finish . . .  [0]
	[INFO] Waiting for the upgrade process to finish . . .  [1]
	[INFO] Stopping the Ops Center Analyzer probe server service...
	[INFO] The Ops Center Analyzer probe server service stopped.
	[INFO] Environment settings have been applied.
	[INFO] Starting the Ops Center Analyzer probe server service...
	[INFO] The Ops Center Analyzer probe server service started.
	[INFO] Starting crond service.
	[INFO] Crond is running.
	[INFO] Installation of the Ops Center Analyzer probe server finished successfully.
	[INFO] The RAID Agent update has started.
	[INFO] Applying environment settings...
	[INFO] Stopping the RAID Agent service...
	[INFO] The RAID Agent service stopped.
	[INFO] Environment settings have been applied.
	[INFO] Starting the RAID Agent service...
	[INFO] The RAID Agent service started.
	[INFO] The RAID Agent update finished successfully.
	
	When you want to monitor the Virtual Storage Software Block, you need to install the Virtual Storage Software Agent server.
	Do you want to install the Virtual Storage Software Agent server? (y/n) [n]: y
	
	[INFO] Hitachi Ops Center Analyzer Virtual Storage Software Agent version 10.8.3-00 installation will now start.
	Specify the directory to store application data. [/opt/hitachi]:
	[INFO] Precheck started.
	==========================================================================
	Ops Center Analyzer Virtual Storage Software Agent Precheck ver. 10.8.3-00
	==========================================================================
	
	[ Check results ]
	Hitachi Ops Center Analyzer Virtual Storage Software Agent [10.8.3-00][OK]
	
	[ Details ]
	Check prerequisite OS version.                                        [OK]
	Check required RPM packages.                                          [OK]
	
	[INFO] Precheck ended.
	[INFO] Pre-process started.
	[INFO] Install the following packages. (java-1.8.0-amazon-corretto-devel 1.8.0_332.b08-1)
	
	[Firewall Parameter]
	------------------------------------------------------------
	Firewalld service is stopped. Skip the firewall configuration.
	Open the port manually after the installation is completed.
	------------------------------------------------------------
	
	[Firewall Confirmation]
	------------------------------------------------------------
	Firewall accept rule to be added      : none (Skip the firewall configuration.)
	------------------------------------------------------------
	
	[Current Installation Destination Settings]
	------------------------------------------------------------
	Installation destination directory:
	  /opt/hitachi/VirtualStorageSoftwareAgent
	Installation destination for user data files:
	  /var/opt/hitachi/VirtualStorageSoftwareAgent
	Installation destination for log files (Fixed):
	  /var/log/hitachi/VirtualStorageSoftwareAgent
	------------------------------------------------------------
	
	Do you want to continue the installation? (y/n) [n]: y
	[INFO] Pre-process ended.
	[INFO] OSS installation starts.
	[INFO] OSS installation ends successfully.
	[INFO] Installing VirtualStorageSoftwareAgent.
	[INFO] Post-process started.
	[INFO] Post-process ended.
	[INFO] Installation ends successfully.
	[INFO] Installation of the Ops Center Analyzer probe servers finished successfully.
	
	
##### 5. Protector
---

	[root@hdsopstml ops_center_update]# mount -o loop HOC10-83-00_E02_PROTECTOR.iso cd
	mount: /dev/loop0 is write-protected, mounting read-only
	[root@hdsopstml ops_center_update]# cd cd
	[root@hdsopstml cd]# ls
	AIX  Linux  SDK  VMware  Windows
	[root@hdsopstml cd]# cd Linux/
	[root@hdsopstml Linux]# ls
	Protector-R7.4-7.4.1.93941-Linux-x64.run  Protector-R7.4-7.4.1.93941-Linux-x64.run.cfg
	[root@hdsopstml Linux]# ./Protector-R7.4-7.4.1.93941-Linux-x64.run
	Are you sure you would like to upgrade? WARNING: ALL unsaved changes will be discarded. Please make sure you save all desired changes before continuing. [Y/n]: Y
	
	----------------------------------------------------------------------------
	Welcome to the Hitachi Ops Center Protector Setup Wizard.
	
	----------------------------------------------------------------------------
	Please read the following License Agreement. You must accept the terms of this
	agreement before continuing with the installation.
	
	Press [Enter] to continue:
	 STANDALONE SOFTWARE LICENSE TERMS 
	
	Unless You have a Direct Purchase Agreement or any other form of supply or
	licensing agreement in place with Hitachi Vantara LLC or its Affiliate
	("HITACHI") or a HITACHI Partner, these Software License Terms ("License Terms")
	apply to the HITACHI software purchased or downloaded by You ("Software"). You
	must read these License Terms under which HITACHI will license Software to You.
	Capitalized terms will have the meanings indicated in Section 21 below.
	
	
	....................
	
	
		  WMS Terms: the Warranty, Maintenance and Support Terms forming part of the
	Online Terms located at
	https://www.hitachivantara.com/en-us/company/legal.terms-licensing-maintenance.ht
	ml and all replacements and updates from time to time.
		  You or Your: the entity with whom HITACHI licenses the Software on these
	License Terms.
	Press [Enter] to continue:
	
	
	
	Standalone Software License Terms v9 February 2020 Global
	Press [Enter] to continue:
	
	Do you accept this license? [y/n]:
	
	Do you accept this license? [y/n]: Y
	
	----------------------------------------------------------------------------
	Upgrade Warning
	
	You are upgrading your installation to version 7.4. Please note:
	
	* If you plan to use the push upgrade mechanism to upgrade the clients, please
	abort this installation and upgrade the clients first. If you continue all
	clients not running 7.4 will show offline and will need to be upgraded using the
	installer on each client server.
	
	* Client nodes upgraded to 7.4 prior to the master will show offline. Once the
	master is upgraded to 7.4, they will show online again.
	
	I understand the above implications of upgrading [y/N]: y
	
	
	----------------------------------------------------------------------------
	Setup is now ready to begin installing Hitachi Ops Center Protector on your
	computer.
	
	Do you want to continue? [Y/n]: Y
	
	----------------------------------------------------------------------------
	Please wait while Setup installs Hitachi Ops Center Protector on your computer.
	
	 Installing
	 0% ________ 50% ________ 100%
	 #########################################
	
	----------------------------------------------------------------------------
	Setup has finished installing Hitachi Ops Center Protector on your computer.
	
	
	
##### 6. Automator
---

	[root@hdsopstml Linux]# cd HAD_SERVER/
	[root@hdsopstml HAD_SERVER]# ls
	REDHAT
	[root@hdsopstml HAD_SERVER]# cd REDHAT/
	[root@hdsopstml REDHAT]# ls
	AGTLESS_LIB  HAD  HADINST  hcmdsdbpdls  hcsversion.properties  hinstsetup.tar  install.sh  instchkstr  LIB  MIGRATIONTOOL  private  public  setup
	[root@hdsopstml REDHAT]# ./install.sh
	Starting Hitachi Ops Center Automator 10.8.3 installation.
	The following product is going to be installed:
	  Hitachi Ops Center Automator 10.8.3-00 [Upgrade]
	
	Caution: Quit all programs. If security, process, and virus monitoring programs are running, the installation can fail.
	
	Press Enter to continue.
	
	KNAE04754-I Hitachi Ops Center Automator and the associated products services must stop during the installation.
	
	Continue? (y/n)
	> y
	
	
	The following product is going to be installed:
	
	<<Information of Hitachi Ops Center Automator product>>
	Installation destination directory.
		/opt/hitachi/Automation
	The storage destination for the database files:
		/var/opt/hitachi/database/x64/Automation
	
	
	<<Information of common component>>
	Installation destination directory.
		/opt/hitachi/Base64
	The storage destination for the database files:
		/var/opt/hitachi/database/x64
	
	Press Enter to continue.
	
	<<Common information>>
	Back up the database before installing: Yes
	  The backup database destination:
		/var/opt/hitachi/Automation_backup
	Set services to start after installation: Yes
	
	To leave things as they are and not make changes, press y.
	To make changes, press e.
	To cancel installation, press n.
	> y
	
	
	The Hitachi Ops Center Automator installation can take several minutes.
	
	Now stopping Hitachi Ops Center Automator and the associated products services...
	Now backing up the database...
	KAPM05900-I The hcmds64dbtrans command has started.
	KAPM05930-I The Application data will now be exported.
	KAPM06525-I The data for HBase will now be moved.
	KAPM06531-I Processing to export data has started.
	KAPM06532-I The table definitions of the database are being exported.
	KAPM06533-I The data is being exported.
	KAPM06537-I The data is being exported. (progress = 001/130)
	KAPM06537-I The data is being exported. (progress = 002/130)
	KAPM06537-I The data is being exported. (progress = 003/130)
	KAPM06537-I The data is being exported. (progress = 004/130)
	KAPM06537-I The data is being exported. (progress = 005/130)
	KAPM06537-I The data is being exported. (progress = 006/130)
	KAPM06537-I The data is being exported. (progress = 007/130)
	KAPM06537-I The data is being exported. (progress = 008/130)
	KAPM06537-I The data is being exported. (progress = 009/130)
	KAPM06537-I The data is being exported. (progress = 010/130)
	KAPM06537-I The data is being exported. (progress = 011/130)
	KAPM06537-I The data is being exported. (progress = 012/130)
	KAPM06537-I The data is being exported. (progress = 013/130)
	KAPM06537-I The data is being exported. (progress = 014/130)
	KAPM06537-I The data is being exported. (progress = 015/130)
	KAPM06537-I The data is being exported. (progress = 016/130)
	KAPM06537-I The data is being exported. (progress = 017/130)
	KAPM06537-I The data is being exported. (progress = 018/130)
	KAPM06537-I The data is being exported. (progress = 019/130)
	KAPM06537-I The data is being exported. (progress = 020/130)
	KAPM06537-I The data is being exported. (progress = 021/130)
	KAPM06537-I The data is being exported. (progress = 022/130)
	KAPM06537-I The data is being exported. (progress = 023/130)
	KAPM06537-I The data is being exported. (progress = 024/130)
	KAPM06537-I The data is being exported. (progress = 025/130)
	KAPM06537-I The data is being exported. (progress = 026/130)
	KAPM06537-I The data is being exported. (progress = 027/130)
	KAPM06537-I The data is being exported. (progress = 028/130)
	KAPM06537-I The data is being exported. (progress = 029/130)
	KAPM06537-I The data is being exported. (progress = 030/130)
	KAPM06537-I The data is being exported. (progress = 031/130)
	KAPM06537-I The data is being exported. (progress = 032/130)
	KAPM06537-I The data is being exported. (progress = 033/130)
	KAPM06537-I The data is being exported. (progress = 034/130)
	KAPM06537-I The data is being exported. (progress = 035/130)
	KAPM06537-I The data is being exported. (progress = 036/130)
	KAPM06537-I The data is being exported. (progress = 037/130)
	KAPM06537-I The data is being exported. (progress = 038/130)
	KAPM06537-I The data is being exported. (progress = 039/130)
	KAPM06537-I The data is being exported. (progress = 040/130)
	KAPM06537-I The data is being exported. (progress = 041/130)
	KAPM06537-I The data is being exported. (progress = 042/130)
	KAPM06537-I The data is being exported. (progress = 043/130)
	KAPM06537-I The data is being exported. (progress = 044/130)
	KAPM06537-I The data is being exported. (progress = 045/130)
	KAPM06537-I The data is being exported. (progress = 046/130)
	KAPM06537-I The data is being exported. (progress = 047/130)
	KAPM06537-I The data is being exported. (progress = 048/130)
	KAPM06537-I The data is being exported. (progress = 049/130)
	KAPM06537-I The data is being exported. (progress = 050/130)
	KAPM06537-I The data is being exported. (progress = 051/130)
	KAPM06537-I The data is being exported. (progress = 052/130)
	KAPM06537-I The data is being exported. (progress = 053/130)
	KAPM06537-I The data is being exported. (progress = 054/130)
	KAPM06537-I The data is being exported. (progress = 055/130)
	KAPM06537-I The data is being exported. (progress = 056/130)
	KAPM06537-I The data is being exported. (progress = 057/130)
	KAPM06537-I The data is being exported. (progress = 058/130)
	KAPM06537-I The data is being exported. (progress = 059/130)
	KAPM06537-I The data is being exported. (progress = 060/130)
	KAPM06537-I The data is being exported. (progress = 061/130)
	KAPM06537-I The data is being exported. (progress = 062/130)
	KAPM06537-I The data is being exported. (progress = 063/130)
	KAPM06537-I The data is being exported. (progress = 064/130)
	KAPM06537-I The data is being exported. (progress = 065/130)
	KAPM06537-I The data is being exported. (progress = 066/130)
	KAPM06537-I The data is being exported. (progress = 067/130)
	KAPM06537-I The data is being exported. (progress = 068/130)
	KAPM06537-I The data is being exported. (progress = 069/130)
	KAPM06537-I The data is being exported. (progress = 070/130)
	KAPM06537-I The data is being exported. (progress = 071/130)
	KAPM06537-I The data is being exported. (progress = 072/130)
	KAPM06537-I The data is being exported. (progress = 073/130)
	KAPM06537-I The data is being exported. (progress = 074/130)
	KAPM06537-I The data is being exported. (progress = 075/130)
	KAPM06537-I The data is being exported. (progress = 076/130)
	KAPM06537-I The data is being exported. (progress = 077/130)
	KAPM06537-I The data is being exported. (progress = 078/130)
	KAPM06537-I The data is being exported. (progress = 079/130)
	KAPM06537-I The data is being exported. (progress = 080/130)
	KAPM06537-I The data is being exported. (progress = 081/130)
	KAPM06537-I The data is being exported. (progress = 082/130)
	KAPM06537-I The data is being exported. (progress = 083/130)
	KAPM06537-I The data is being exported. (progress = 084/130)
	KAPM06537-I The data is being exported. (progress = 085/130)
	KAPM06537-I The data is being exported. (progress = 086/130)
	KAPM06537-I The data is being exported. (progress = 087/130)
	KAPM06537-I The data is being exported. (progress = 088/130)
	KAPM06537-I The data is being exported. (progress = 089/130)
	KAPM06537-I The data is being exported. (progress = 090/130)
	KAPM06537-I The data is being exported. (progress = 091/130)
	KAPM06537-I The data is being exported. (progress = 092/130)
	KAPM06537-I The data is being exported. (progress = 093/130)
	KAPM06537-I The data is being exported. (progress = 094/130)
	KAPM06537-I The data is being exported. (progress = 095/130)
	KAPM06537-I The data is being exported. (progress = 096/130)
	KAPM06537-I The data is being exported. (progress = 097/130)
	KAPM06537-I The data is being exported. (progress = 098/130)
	KAPM06537-I The data is being exported. (progress = 099/130)
	KAPM06537-I The data is being exported. (progress = 100/130)
	KAPM06537-I The data is being exported. (progress = 101/130)
	KAPM06537-I The data is being exported. (progress = 102/130)
	KAPM06537-I The data is being exported. (progress = 103/130)
	KAPM06537-I The data is being exported. (progress = 104/130)
	KAPM06537-I The data is being exported. (progress = 105/130)
	KAPM06537-I The data is being exported. (progress = 106/130)
	KAPM06537-I The data is being exported. (progress = 107/130)
	KAPM06537-I The data is being exported. (progress = 108/130)
	KAPM06537-I The data is being exported. (progress = 109/130)
	KAPM06537-I The data is being exported. (progress = 110/130)
	KAPM06537-I The data is being exported. (progress = 111/130)
	KAPM06537-I The data is being exported. (progress = 112/130)
	KAPM06537-I The data is being exported. (progress = 113/130)
	KAPM06537-I The data is being exported. (progress = 114/130)
	KAPM06537-I The data is being exported. (progress = 115/130)
	KAPM06537-I The data is being exported. (progress = 116/130)
	KAPM06537-I The data is being exported. (progress = 117/130)
	KAPM06537-I The data is being exported. (progress = 118/130)
	KAPM06537-I The data is being exported. (progress = 119/130)
	KAPM06537-I The data is being exported. (progress = 120/130)
	KAPM06537-I The data is being exported. (progress = 121/130)
	KAPM06537-I The data is being exported. (progress = 122/130)
	KAPM06537-I The data is being exported. (progress = 123/130)
	KAPM06537-I The data is being exported. (progress = 124/130)
	KAPM06537-I The data is being exported. (progress = 125/130)
	KAPM06537-I The data is being exported. (progress = 126/130)
	KAPM06537-I The data is being exported. (progress = 127/130)
	KAPM06537-I The data is being exported. (progress = 128/130)
	KAPM06537-I The data is being exported. (progress = 129/130)
	KAPM06537-I The data is being exported. (progress = 130/130)
	KAPM06525-I The data for Automation will now be moved.
	KAPM06531-I Processing to export data has started.
	KAPM06532-I The table definitions of the database are being exported.
	KAPM06533-I The data is being exported.
	KAPM06537-I The data is being exported. (progress = 01/63)
	KAPM06537-I The data is being exported. (progress = 02/63)
	KAPM06537-I The data is being exported. (progress = 03/63)
	KAPM06537-I The data is being exported. (progress = 04/63)
	KAPM06537-I The data is being exported. (progress = 05/63)
	KAPM06537-I The data is being exported. (progress = 06/63)
	KAPM06537-I The data is being exported. (progress = 07/63)
	KAPM06537-I The data is being exported. (progress = 08/63)
	KAPM06537-I The data is being exported. (progress = 09/63)
	KAPM06537-I The data is being exported. (progress = 10/63)
	KAPM06537-I The data is being exported. (progress = 11/63)
	KAPM06537-I The data is being exported. (progress = 12/63)
	KAPM06537-I The data is being exported. (progress = 13/63)
	KAPM06537-I The data is being exported. (progress = 14/63)
	KAPM06537-I The data is being exported. (progress = 15/63)
	KAPM06537-I The data is being exported. (progress = 16/63)
	KAPM06537-I The data is being exported. (progress = 17/63)
	KAPM06537-I The data is being exported. (progress = 18/63)
	KAPM06537-I The data is being exported. (progress = 19/63)
	KAPM06537-I The data is being exported. (progress = 20/63)
	KAPM06537-I The data is being exported. (progress = 21/63)
	KAPM06537-I The data is being exported. (progress = 22/63)
	KAPM06537-I The data is being exported. (progress = 23/63)
	KAPM06537-I The data is being exported. (progress = 24/63)
	KAPM06537-I The data is being exported. (progress = 25/63)
	KAPM06537-I The data is being exported. (progress = 26/63)
	KAPM06537-I The data is being exported. (progress = 27/63)
	KAPM06537-I The data is being exported. (progress = 28/63)
	KAPM06537-I The data is being exported. (progress = 29/63)
	KAPM06537-I The data is being exported. (progress = 30/63)
	KAPM06537-I The data is being exported. (progress = 31/63)
	KAPM06537-I The data is being exported. (progress = 32/63)
	KAPM06537-I The data is being exported. (progress = 33/63)
	KAPM06537-I The data is being exported. (progress = 34/63)
	KAPM06537-I The data is being exported. (progress = 35/63)
	KAPM06537-I The data is being exported. (progress = 36/63)
	KAPM06537-I The data is being exported. (progress = 37/63)
	KAPM06537-I The data is being exported. (progress = 38/63)
	KAPM06537-I The data is being exported. (progress = 39/63)
	KAPM06537-I The data is being exported. (progress = 40/63)
	KAPM06537-I The data is being exported. (progress = 41/63)
	KAPM06537-I The data is being exported. (progress = 42/63)
	KAPM06537-I The data is being exported. (progress = 43/63)
	KAPM06537-I The data is being exported. (progress = 44/63)
	KAPM06537-I The data is being exported. (progress = 45/63)
	KAPM06537-I The data is being exported. (progress = 46/63)
	KAPM06537-I The data is being exported. (progress = 47/63)
	KAPM06537-I The data is being exported. (progress = 48/63)
	KAPM06537-I The data is being exported. (progress = 49/63)
	KAPM06537-I The data is being exported. (progress = 50/63)
	KAPM06537-I The data is being exported. (progress = 51/63)
	KAPM06537-I The data is being exported. (progress = 52/63)
	KAPM06537-I The data is being exported. (progress = 53/63)
	KAPM06537-I The data is being exported. (progress = 54/63)
	KAPM06537-I The data is being exported. (progress = 55/63)
	KAPM06537-I The data is being exported. (progress = 56/63)
	KAPM06537-I The data is being exported. (progress = 57/63)
	KAPM06537-I The data is being exported. (progress = 58/63)
	KAPM06537-I The data is being exported. (progress = 59/63)
	KAPM06537-I The data is being exported. (progress = 60/63)
	KAPM06537-I The data is being exported. (progress = 61/63)
	KAPM06537-I The data is being exported. (progress = 62/63)
	KAPM06537-I The data is being exported. (progress = 63/63)
	KAPM06541-I The view is being exported.
	KAPM05931-I The Application data has been exported.
	KAPM05930-I The Authentication data will now be exported.
	KAPM05821-I Processing to export data has started.
	KAPM05822-I Processing to export data has ended.
	KAPM05931-I The Authentication data has been exported.
	KAPM05933-I The archive file will now be created.
	KAPM05934-I The archive file has been created.
	KAPM05901-I The hcmds64dbtrans command ended normally.
	The Hitachi Ops Center Automator installation is started. Please do not interrupt.
	Now installing Common Component...
	Now installing Hitachi Ops Center Automator...
	Now performing post-installation processing...
	The installation processing has been completed.
	
	Now configuring the Hitachi Ops Center Automator server...
	This process may take some time.
	Now starting Hitachi Ops Center Automator and the associated products services...
	Hitachi Ops Center Automator installation completed successfully.
