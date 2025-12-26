### SOM
---



#### Raidcom
	raidcom modify system_opt -system_option_mode system -mode_id <SOM> -mode enable -Ixxx
	raidcom modify system_opt -system_option_mode system -mode_id <SOM> -mode enable -password <One Time Password> -Ixxx
	raidcom get system_opt -key mode -lpr system -Ixxx


#### SVP
1. Close the all SVP menu.

2. <Enter the password>
	Press [Shift] + [Ctrl] + [m] in the ‘SVP’ window.
	Enter the password, and select (CL) [OK].
	(Please call Technical Support Division for asking it.)
	CAUTION: This is a special (exceptional) operation that requires an input of a password. Ask the technical support division and input the password.

	![som01.png](../.images/som01.png)

3. <Mode Mode>
	‘Mode Mode’ is displayed.
	Select (CL) [Install].

	![som02.png](../.images/som02.png)

4. <Install Window>
	Select (CL) the [Change Configuration] menu
	in the ‘Install’ window.

	![som03.png](../.images/som03.png)

5. <Menu Dialog Window>
	Select (CL) the [System Option] menu in the
	‘Menu Dialog’ window and select (CL) [OK].

	![som04.png](../.images/som04.png)

6. <System Option Dialog>
	Select (CL) [Mode...] in the ‘System Option’. Go to Step (7).
	When the setting of all the entry items is completed, select (CL) [OK]. Go to Step (8).

	![som05.png](../.images/som05.png)

7. <Mode Window >
	Select (CL) [LPR] and [Mode Configuration] in the ‘Mode’ window and select (CL) [OK].
	Return to Step (6).
	
	[LPR] : Select the following item.
		System: Apply to the whole system.
		LPR0 - LPR31 : Apply to the CLPR0 - CLPR31.
		
	The following is definition of each Mode Class.
		P (Public) : Any permission is unnecessary.
		S (TS) : The permission of the Technical Support Division is necessary. When you select (CL) the check box for “S”, go to the Step (7-1).
		R (Hitachi, Ltd.) : The permission of Hitachi, Ltd is necessary. When you select (CL) the check box for “R”, go to the Step (7-2).
	
	NOTE:You can change the display range shown in [Mode Configuration] in the “Range of Mode” combo box.

	![som06.png](../.images/som06.png)

    1. “If you want to change this Local Mode, permission from the Technical Support Division is required. If you already have the permission, select [Yes].” is displayed.
		 When you select (CL) [Yes], the settings are included. Go back to the Step (7). When you select (CL) [No], the settings are not included. Go back to the Step (7).
		![som07.png](../.images/som07.png)

	2. “A password is required to set this Local Mode. Do you want to continue?” is displayed.
		 When you select (CL) [Yes], go to the Step (7-2-1). When you select (CL) [No], go back to the Step (7).
		![som08.png](../.images/som08.png)
  
		1. Enter the password and select (CL) [OK].
			 Go back to the Step (7). 
			 Entering the password is required in this operation. Please call Technical Support Division for asking it.
			![som09.png](../.images/som09.png)
8. “Change Configuration was completed.” is displayed.
	 Select (CL) [OK].
    ![som10.png](../.images/som10.png)

9. Execute an operation for backing up the configuration information.
	 Prepare the removable media for backup and insert the media.
	 Please select (CL) the [Refresh] button, and update drive information.

	 **NOTE**:For the procedure of backing up the configuration information to a media, see page MICRO07-190.
	![som11.png](../.images/som11.png)
   
10. When this procedure is completed, the message “Please remove the Config media.” is displayed.
      Remove the configuration information media, select (CL) [OK].
	  ![som12.png](../.images/som12.png)

11. Close the ‘Install’ window.
	  Select (CL) [File]-[Exit].
	  ![som13.png](../.images/som13.png)

12. Change the Mode from [Mode Mode] to [View Mode].