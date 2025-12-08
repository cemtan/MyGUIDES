### 3DC SETUP
---
---



#### 1. ADD REMOTE CONNECTIONS
---

##### 1. Change the attribute of ports to Bidirectional (//: Do not run for E1090)
	// raidcom modify port -port CL1-E -port_attribute ALL -IH0
	// raidcom modify port -port CL2-E -port_attribute ALL -IH0
	// raidcom modify port -port CL3-E -port_attribute ALL -IH0
	// raidcom modify port -port CL4-E -port_attribute ALL -IH0
	// raidcom modify port -port CL1-E -port_attribute ALL -IH1
	// raidcom modify port -port CL2-E -port_attribute ALL -IH1
	// raidcom modify port -port CL3-E -port_attribute ALL -IH1
	// raidcom modify port -port CL4-E -port_attribute ALL -IH1
	// raidcom modify port -port CL1-E -port_attribute ALL -IH2
	// raidcom modify port -port CL2-E -port_attribute ALL -IH2
	// raidcom modify port -port CL3-E -port_attribute ALL -IH2
	// raidcom modify port -port CL4-E -port_attribute ALL -IH2
	// raidcom modify port -port CL1-E -port_attribute ALL -IH2
	// raidcom modify port -port CL2-E -port_attribute ALL -IH2
	// raidcom modify port -port CL3-E -port_attribute ALL -IH2
	// raidcom modify port -port CL4-E -port_attribute ALL -IH2

Display port information

	raidcom get port -IH2

##### 2. Add a remote connection with path group ID 0
###### FIBER
Add a remote connection for HUR with path group ID 1

	raidcom add rcu -cu_free 715061 M800 1 -mcu_port CL1-E -rcu_port CL1-E -IH0
	raidcom add rcu -cu_free 745175 M800 1 -mcu_port CL1-E -rcu_port CL1-E -IH2

Add a remote connection for DELTA with path group ID 2

	raidcom add rcu -cu_free 715061 M800 2 -mcu_port CL1-E -rcu_port CL1-E -IH1
	raidcom add rcu -cu_free 715062 M800 2 -mcu_port CL1-E -rcu_port CL1-E -IH2

###### ISCSI
Add a remote connection for HUR with path group ID 1

	raidcom add rcu_iscsi_port -port CL1-E -rcu_port CL1-E -rcu_id 715061 M800 -rcu_address 172.30.40.120 -IH0
	raidcom add rcu_iscsi_port -port CL2-E -rcu_port CL2-E -rcu_id 715061 M800 -rcu_address 172.30.40.121 -IH0
	raidcom add rcu_iscsi_port -port CL1-E -rcu_port CL3-E -rcu_id 715061 M800 -rcu_address 172.30.40.122 -IH0
	raidcom add rcu_iscsi_port -port CL2-E -rcu_port CL4-E -rcu_id 715061 M800 -rcu_address 172.30.40.123 -IH0
	raidcom add rcu_iscsi_port -port CL1-E -rcu_port CL3-E -rcu_id 745175 M800 -rcu_address 172.30.140.120 -IH2
	raidcom add rcu_iscsi_port -port CL2-E -rcu_port CL4-E -rcu_id 745175 M800 -rcu_address 172.30.140.121 -IH2
	raidcom add rcu_iscsi_port -port CL3-E -rcu_port CL3-E -rcu_id 745175 M800 -rcu_address 172.30.140.122 -IH2
	raidcom add rcu_iscsi_port -port CL4-E -rcu_port CL4-E -rcu_id 745175 M800 -rcu_address 172.30.140.123 -IH2

Add a remote connection for DELTA with path group ID 2

	raidcom add rcu_iscsi_port -port CL1-E -rcu_port CL1-E -rcu_id 715061 M800 -rcu_address 172.30.40.120 -IH1
	raidcom add rcu_iscsi_port -port CL2-E -rcu_port CL2-E -rcu_id 715061 M800 -rcu_address 172.30.40.121 -IH1
	raidcom add rcu_iscsi_port -port CL1-E -rcu_port CL3-E -rcu_id 715061 M800 -rcu_address 172.30.40.122 -IH1
	raidcom add rcu_iscsi_port -port CL2-E -rcu_port CL4-E -rcu_id 715061 M800 -rcu_address 172.30.40.123 -IH1
	raidcom add rcu_iscsi_port -port CL1-E -rcu_port CL3-E -rcu_id 715062 M800 -rcu_address 172.30.140.126 -IH2
	raidcom add rcu_iscsi_port -port CL2-E -rcu_port CL4-E -rcu_id 715062 M800 -rcu_address 172.30.140.127 -IH2
	raidcom add rcu_iscsi_port -port CL3-E -rcu_port CL3-E -rcu_id 715062 M800 -rcu_address 172.30.140.128 -IH2
	raidcom add rcu_iscsi_port -port CL4-E -rcu_port CL4-E -rcu_id 715062 M800 -rcu_address 172.30.140.129 -IH2

Confirm that asynchronous command processing has completed

	raidcom get command_status -IH0
	raidcom get command_status -IH1
	raidcom get command_status -IH2

Display remote connection information

	raidcom get rcu_iscsi_port -IH2
	raidcom get rcu_iscsi_port -IH0
	raidcom get rcu_iscsi_port -IH1

Test connection

	raidcom send ping -port CL1-E -address 172.30.40.120 -IH0
	raidcom send ping -port CL1-E -address 172.30.40.120 -IH1
	raidcom send ping -port CL1-E -address 172.30.140.120 -IH2
	raidcom send ping -port CL1-E -address 172.30.140.126 -IH2



#### 2. CREATE & CONFIGURE JOURNAL
---

##### 1. Create Journal Volume
	raidcom add ldev -pool 0 -ldev_id 0x3F10 -capacity 2T -IH0
	raidcom add ldev -pool 0 -ldev_id 0x3F10 -capacity 2T -IH1
	raidcom add ldev -pool 0 -ldev_id 0x3F10 -capacity 2T -IH2

Confirm that asynchronous command processing has completed

	raidcom get command_status -IH0
	raidcom get command_status -IH1
	raidcom get command_status -IH2

Check Journal LDEV

	raidcom get ldev -ldev_id 0x3F10 -fx -IH0
	raidcom get ldev -ldev_id 0x3F10 -fx -IH1
	raidcom get ldev -ldev_id 0x3F10 -fx -IH2

##### 2. Add journal with id 0
	raidcom add journal -journal_id 0 -ldev_id 0x3F10 -IH0
	raidcom add journal -journal_id 0 -ldev_id 0x3F10 -IH1
	raidcom add journal -journal_id 0 -ldev_id 0x3F10 -IH2

Modify journals for fast transfer

	raidcom modify journal -journal_id 0 -mirror_id 0 -copy_size 3 -IH0
	raidcom modify journal -journal_id 0 -mirror_id 1 -copy_size 3 -IH0
	raidcom modify journal -journal_id 0 -mirror_id 2 -copy_size 3 -IH0
	raidcom modify journal -journal_id 0 -mirror_id 3 -copy_size 3 -IH0
	raidcom modify journal -journal_id 0 -data_overflow_watch 0 -IH0
	
	raidcom modify journal -journal_id 0 -mirror_id 0 -copy_size 3 -IH1
	raidcom modify journal -journal_id 0 -mirror_id 1 -copy_size 3 -IH1
	raidcom modify journal -journal_id 0 -mirror_id 2 -copy_size 3 -IH1
	raidcom modify journal -journal_id 0 -mirror_id 3 -copy_size 3 -IH1
	raidcom modify journal -journal_id 0 -data_overflow_watch 0 -IH1
	
	raidcom modify journal -journal_id 0 -mirror_id 0 -copy_size 3 -IH2
	raidcom modify journal -journal_id 0 -mirror_id 1 -copy_size 3 -IH2
	raidcom modify journal -journal_id 0 -mirror_id 2 -copy_size 3 -IH2
	raidcom modify journal -journal_id 0 -mirror_id 3 -copy_size 3 -IH2
	raidcom modify journal -journal_id 0 -data_overflow_watch 0 -IH2

Confirm LDEV is registered to the journal

	raidcom get journal -IH0
	raidcom get journal -IH1
	raidcom get journal -IH2



#### 3. CREATE & CONFIGURE HOST GROUPS
---

##### Cluster 1
	raidcom add host_grp -port CL1-A-1 -host_grp_name ovsintps11 -IH2
	raidcom modify host_grp -port CL1-A-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL1-A-1 -hba_wwn 51402ec0181d98c6 -IH2
	raidcom set hba_wwn -port CL1-A-1 -hba_wwn 51402ec0181d98c6 -wwn_nickname ovsintps11_P0_1 -IH2
	raidcom add host_grp -port CL6-A-1 -host_grp_name ovsintps11 -IH2
	raidcom modify host_grp -port CL6-A-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL6-A-1 -hba_wwn 51402ec0181d9c9a -IH2
	raidcom set hba_wwn -port CL6-A-1 -hba_wwn 51402ec0181d9c9a -wwn_nickname ovsintps11_P1_1 -IH2
	
	raidcom add host_grp -port CL1-B-1 -host_grp_name ovsintps12 -IH2
	raidcom modify host_grp -port CL1-B-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL1-B-1 -hba_wwn 51402ec0181d9416 -IH2
	raidcom set hba_wwn -port CL1-B-1 -hba_wwn 51402ec0181d9416 -wwn_nickname ovsintps12_P0_1 -IH2
	raidcom add host_grp -port CL6-B-1 -host_grp_name ovsintps12 -IH2
	raidcom modify host_grp -port CL6-B-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL6-B-1 -hba_wwn 51402ec0181d9c5a -IH2
	raidcom set hba_wwn -port CL6-B-1 -hba_wwn 51402ec0181d9c5a -wwn_nickname ovsintps12_P1_1 -IH2
	
	raidcom add host_grp -port CL1-D-1 -host_grp_name ovsintps13 -IH2
	raidcom modify host_grp -port CL1-D-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL1-D-1 -hba_wwn 51402ec0181d9cbe -IH2
	raidcom set hba_wwn -port CL1-D-1 -hba_wwn 51402ec0181d9cbe -wwn_nickname ovsintps13_P0_1 -IH2
	raidcom add host_grp -port CL6-D-1 -host_grp_name ovsintps13 -IH2
	raidcom modify host_grp -port CLD-B-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL6-D-1 -hba_wwn 51402ec0181d9c42 -IH2
	raidcom set hba_wwn -port CL6-D-1 -hba_wwn 51402ec0181d9c42 -wwn_nickname ovsintps13_P1_1 -IH2

##### Cluster 2
	raidcom add host_grp -port CL2-A-1 -host_grp_name ovsintps21 -IH2
	raidcom modify host_grp -port CL2-A-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL2-A-1 -hba_wwn 51402ec0181d996a -IH2
	raidcom set hba_wwn -port CL2-A-1 -hba_wwn 51402ec0181d996a -wwn_nickname ovsintps21_P0_1 -IH2
	raidcom add host_grp -port CL5-A-1 -host_grp_name ovsintps21 -IH2
	raidcom modify host_grp -port CL5-A-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL5-A-1 -hba_wwn 51402ec0181d9752 -IH2
	raidcom set hba_wwn -port CL5-A-1 -hba_wwn 51402ec0181d9752 -wwn_nickname ovsintps21_P1_1 -IH2
	
	raidcom add host_grp -port CL2-B-1 -host_grp_name ovsintps22 -IH2
	raidcom modify host_grp -port CL2-B-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL2-B-1 -hba_wwn 51402ec0181d9a76 -IH2
	raidcom set hba_wwn -port CL2-B-1 -hba_wwn 51402ec0181d9a76 -wwn_nickname ovsintps22_P0_1 -IH2
	raidcom add host_grp -port CL5-B-1 -host_grp_name ovsintps22 -IH2
	raidcom modify host_grp -port CL5-B-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL5-B-1 -hba_wwn 51402ec0181d9cca -IH2
	raidcom set hba_wwn -port CL5-B-1 -hba_wwn 51402ec0181d9cca -wwn_nickname ovsintps22_P1_1 -IH2
	
	raidcom add host_grp -port CL2-D-1 -host_grp_name ovsintps23 -IH2
	raidcom modify host_grp -port CL2-D-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL2-D-1 -hba_wwn 51402ec0181d9434 -IH2
	raidcom set hba_wwn -port CL2-D-1 -hba_wwn 51402ec0181d9434 -wwn_nickname ovsintps23_P0_1 -IH2
	raidcom add host_grp -port CL5-D-1 -host_grp_name ovsintps23 -IH2
	raidcom modify host_grp -port CL5-D-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL5-D-1 -hba_wwn 51402ec0181d9812 -IH2
	raidcom set hba_wwn -port CL5-D-1 -hba_wwn 51402ec0181d9812 -wwn_nickname ovsintps23_P1_1 -IH2

##### Cluster 3
	raidcom add host_grp -port CL3-A-1 -host_grp_name ovsdmzps11 -IH2
	raidcom modify host_grp -port CL3-A-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL3-A-1 -hba_wwn 51402ec0181d9cc2 -IH2
	raidcom set hba_wwn -port CL3-A-1 -hba_wwn 51402ec0181d9cc2 -wwn_nickname ovsdmzps11_P0_1 -IH2
	raidcom add host_grp -port CL8-A-1 -host_grp_name ovsdmzps11 -IH2
	raidcom modify host_grp -port CL8-A-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL8-A-1 -hba_wwn 51402ec0181d93e2 -IH2
	raidcom set hba_wwn -port CL8-A-1 -hba_wwn 51402ec0181d93e2 -wwn_nickname ovsdmzps11_P1_1 -IH2
	
	raidcom add host_grp -port CL3-B-1 -host_grp_name ovsdmzps12 -IH2
	raidcom modify host_grp -port CL3-B-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL3-B-1 -hba_wwn 51402ec0181d9822 -IH2
	raidcom set hba_wwn -port CL3-B-1 -hba_wwn 51402ec0181d9822 -wwn_nickname ovsdmzps12_P0_1 -IH2
	raidcom add host_grp -port CL8-B-1 -host_grp_name ovsdmzps12 -IH2
	raidcom modify host_grp -port CL8-B-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL8-B-1 -hba_wwn 51402ec0181d9c6a -IH2
	raidcom set hba_wwn -port CL8-B-1 -hba_wwn 51402ec0181d9c6a -wwn_nickname ovsdmzps12_P1_1 -IH2
	
	raidcom add host_grp -port CL3-D-1 -host_grp_name ovsdmzps13 -IH2
	raidcom modify host_grp -port CL3-D-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL3-D-1 -hba_wwn 51402ec0181d9c1e -IH2
	raidcom set hba_wwn -port CL3-D-1 -hba_wwn 51402ec0181d9c1e -wwn_nickname ovsdmzps13_P0_1 -IH2
	raidcom add host_grp -port CL8-D-1 -host_grp_name ovsdmzps13 -IH2
	raidcom modify host_grp -port CL8-D-1 -host_mode VMWARE_EX -IH2
	raidcom add hba_wwn -port CL8-D-1 -hba_wwn 51402ec0181d93ea -IH2
	raidcom set hba_wwn -port CL8-D-1 -hba_wwn 51402ec0181d93ea -wwn_nickname ovsdmzps13_P1_1 -IH2



#### 4. CREATE & ASSIGN VOLUMES
---

##### 1. Create Volumes
	raidcom add ldev -pool 0 -ldev_id 0 -capacity 4T -capacity_saving deduplication_compression -IH2
	raidcom modify ldev -ldev_id 0 -ldev_name BVMINT02_POOL01 -IH2
	raidcom get ldev -ldev_id 0 -fx -IH2

##### 2. Add Luns
	raidcom add lun -port CL1-A-1 -lun_id 0 -ldev_id 0 -IH2
	raidcom add lun -port CL1-B-1 -lun_id 0 -ldev_id 0 -IH2
	raidcom add lun -port CL1-D-1 -lun_id 0 -ldev_id 0 -IH2
	raidcom get lun -port CL1-A-1 -fx -IH2
	raidcom get lun -port CL1-B-1 -fx -IH2
	raidcom get lun -port CL1-D-1 -fx -IH2
	
	raidcom add lun -port CL6-A-1 -lun_id 0 -ldev_id 0 -IH2
	raidcom add lun -port CL6-B-1 -lun_id 0 -ldev_id 0 -IH2
	raidcom add lun -port CL6-D-1 -lun_id 0 -ldev_id 0 -IH2
	raidcom get lun -port CL6-A-1 -fx -IH2
	raidcom get lun -port CL6-B-1 -fx -IH2
	raidcom get lun -port CL6-D-1 -fx -IH2



#### 5. RECONFIGURE HORCM FILES for 3DC
---

##### 1. Stop Instances
	horcmshutdown 0 1 2

##### 2. Reconfigure and Start Instances
	horcmstart 0 1 2
	raidcom -login horcm Passw00rd -IH0
	raidcom -login horcm Passw00rd -IH1
	raidcom -login horcm Passw00rd -IH2



#### 6. CREATE 3DC PAIRS
---

##### 1. Create DELTA pair first
	paircreate -g bvmint02_DLT -f async -vl -jp 0 -js 0 -nocsus –IH1

##### 2. Confirm that the UR delta resync pair creation is completed  
In CCI, the pair status of P-VOL is displayed as PSUE, and the mirror status of the journal is displayed as PJNS.

	pairdisplay -g bvmint02_DLT -fxce –IH1

##### 3. Create HUR pair finally
	paircreate -g bvmint02_HUR -f async -vl -jp 0 -js 0 –IH0

##### 4. Check replication status
	pairdisplay -g bvmint02_HUR -fxce –IH0
