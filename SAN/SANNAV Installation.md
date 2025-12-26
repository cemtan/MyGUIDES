### SANNAV INSTALLATION
---
---
***



#### How to configure SANNAV event notifications and Email setup
---

- Ensure that email server is an IP address or an FQDN
- SANnav supports filtering email notifications that are based on event rules and switches. You cannot receive the event notification unless the administrator has enabled email event notification in the **User Preferences** tab.
- To receive email notifications, you must set the duration to receive the event emails on the **SANnav Email Setup** page and configure the email ID in the **SANnav User Management** under **Security**.

_https://www.youtube.com/watch?v=CdTbPSC5D5o_

1. Log in to the SANnav Management Portal.
2. Click **SANnav → Services → SANnav Email Setup**.
	* Ensure that the email server is an IP address or an FQDN.
	* Test an email account accessible to the SANnav Administrator.
	* Ensure that the "Enable" button is selected and the changes are saved.

	![san_img04.png](../.images/san_img04.png)

3. Enable Email Event Notifications by clicking upper-right corner of the **UI → User Preferences**.
	* Click Edit button next to Notifications.
	* Select Enable Email Event Notifications then press **SAVE**.

	![san_img05.png](../.images/san_img05.png)

4. Enable event notification intervals by clicking **SANnav → Event Management → Event Notification**.
	**Note**: This defines the interval on how often SANnav sends the bulk email to the SAN admins.
	* Select Enable then press "**SAVE**."
  
	![san_img06.png](../.images/san_img06.png)


5. Configure email accounts for each SANnav local user. 
	* SANnav does not support email setup for remote authentication users on LDAP server.
	* Goto **SANNAV → Security → SANNAV User Management**.
	* For each local user, click the username then edit the Email section and add in the email address. For most customers, the Administrator username might be the best option since it is always there by default.
	* For each local user account, the above must be done - either from Administrator or that specific user once logged in.
	*  A non-Administrator user cannot make changes to any email settings except their own. They must be done from Administrator.
	* For LDAP users, you see the account that is listed as "External" in the Users menu (no email setup is possible for these remote users):

	![san_img07.png](../.images/san_img07.png)



#### Registering for SNMP Traps and Syslog Recipients
---

**Fault Management > SNMP and Syslog Management**

![san_img08.png](../.images/san_img08.png)



#### Notifying Health Status Changes
---

**Fault Management > Event Policy Management**


![san_img09.png](../.images/san_img09.png)



#### Creating an Event Management Dashboards
---

##### Status
![san_img10.png](../.images/san_img10.png)

##### Performance
![san_img11.png](../.images/san_img11.png)



 #### Creating an Event Management Reports
---

##### Status
![san_img12.png](../.images/san_img12.png)

##### Performance - Time Series - Chasis Utilization
![san_img13.png](../.images/san_img13.png) ![san_img14.png](../.images/san_img14.png)
Top port BB Credit Zero: Loses one buffer for each frame acknowledgement failure


Set access control if Violations are not shown:

	snmpconfig --show accesscontrol
	snmpconfig --set accesscontrol




#### Configuration
---

* Creating a Configuration Policy

 

#### Monitoring and Alerting Policy Suite
---

* Managing MAPS Actions on a Switch: An active and valid Fabric Vision license is required to configure and view MAPS policies.


