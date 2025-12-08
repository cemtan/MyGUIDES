#### MANAGEMENT
---
---



##### Starting or stopping the Common Services service
	systemctl <start|stop|restart> csportal

##### Changing the management server host name, IP address, or port number
	Log in to the management server as the root user.
	Run following command
		/opt/hitachi/CommonService/utility/bin/cschgconnect.sh [-h host-name-or-IP-address] [-p port-number] | -enableip {true|false} | -list
		
##### Changing the port number used for internal communications
	Refer to Hitachi OPS Center Installation and Configuration Guide.pdf

##### Backing up Common Services
	Log in to the management server as the root user.
	Stop the Common Services service.
	Run the csbackup command.
		/opt/hitachi/CommonService/utility/bin/csbackup.sh -dir backup-destination-directory
		
##### Restoring Common Services
	Log in to the management server as the root user.
	Stop the Common Services service.
	Run the csrestore command.
		/opt/hitachi/CommonService/utility/bin/csrestore.sh -file path-to-backup-file
		
##### Stopping unnecessary product services
	Log in to the management server as the root user.
	Run the opsvmservicectl command to stop the services for the products that are not needed.
		{disable|enable} product-name [product-name ...]
			- Hitachi Ops Center Automator: Automator
			- Hitachi Ops Center Analyzer: Analyzer
			- Hitachi Ops Center Analyzer viewpoint: AnalyzerDetailView
			- Hitachi Ops Center API Configuration Manager: APIConfigurationManage
			- Hitachi Ops Center Protector: Protector
			- Hitachi Ops Center Administrator: Administrator
			- Hitachi Ops Center Common Services: CommonServices
			
##### Resetting the trust relationship with each product
	Log in to the management server as the root user.
	Run the csresettrustrelationship command.
		/opt/hitachi/CommonService/utility/bin/csresettrustrelationship.sh -f
		
##### Upgrading Amazon Corretto 8, PostgreSQL 11
	Refer to Hitachi OPS Center Installation and Configuration Guide.pdf

 ##### Updating OS, docker
	Refer to Hitachi Ops Center Administrator Getting Started Guide.pdf
	
##### Upgrading Ops Center Administrator
	3.4.x > 10.0.x>10.1.x > 10.2.0 > 10.3.x > 10.5.x > 10.6.0
	Refer to Hitachi Ops Center Administrator Getting Started Guide.pdf
	
##### Change the maximum amount of memory that can be used by the Analyzer server.
	Reference topics - Hitachi Vantara Knowledge
		Changememory {-set memory-size [-auto] | -status}
		
##### Installing Protector Client on probe server
		cd /opt/OpsVM/tmp
		or it is in Protector ISO ( https://support.hitachivantara.com/en/user/answers/downloads/downloads-detail.html?d=Ops%20Center%20Protector&pptype=Software%20Version )
			Hitachi Ops Center Protector Software Distribution Library
		./Protector-R7.1-7.1.2.86633-Linux-x64.run --unattendedmodeui none --mode unattended --node-type client --node-name <nodename> --master-name <mastername>

