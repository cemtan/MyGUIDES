### GAD SETUP
---
---



#### 1. ADD REMOTE CONNECTIONS
---

##### 1. Change the attribute of ports to Bidirectional (//: Do not run for E1090)
	// raidcom modify port -port CL1-B -port_attribute ALL -IH0
	// raidcom modify port -port CL6-B -port_attribute ALL -IH0
	// raidcom modify port -port CL4-D -port_attribute ALL -IH0
	// raidcom modify port -port CL7-D -port_attribute ALL -IH0
	// raidcom modify port -port CL1-B -port_attribute ALL -IH1
	// raidcom modify port -port CL7-D -port_attribute ALL -IH1
	// raidcom modify port -port CL6-B -port_attribute ALL -IH1
	// raidcom modify port -port CL4-D -port_attribute ALL -IH1

Display port information

	raidcom get port -IH0
	raidcom get port -IH1

##### 2. Add a remote connection with path group ID 0
	raidcom add rcu -cu_free 715062 M800 0 -mcu_port CL1-B -rcu_port CL1-B -IH0
	raidcom add rcu -cu_free 715062 M800 0 -mcu_port CL7-D -rcu_port CL7-D -IH0
	raidcom add rcu -cu_free 715062 M800 0 -mcu_port CL6-B -rcu_port CL6-B -IH0
	raidcom add rcu -cu_free 715062 M800 0 -mcu_port CL4-D -rcu_port CL4-D -IH0
	raidcom add rcu -cu_free 745175 M800 0 -mcu_port CL1-B -rcu_port CL1-B -IH1
	raidcom add rcu -cu_free 745175 M800 0 -mcu_port CL7-D -rcu_port CL7-D -IH1
	raidcom add rcu -cu_free 745175 M800 0 -mcu_port CL6-B -rcu_port CL6-B -IH1
	raidcom add rcu -cu_free 745175 M800 0 -mcu_port CL4-D -rcu_port CL4-D -IH1

Confirm that asynchronous command processing has completed

	raidcom get command_status -IH0
	raidcom get command_status -IH1
	
Display remote connection information

	raidcom get rcu -cu_free 715062 M800 0 -IH0
	raidcom get rcu -cu_free 745175 M800 0 -IH1



#### 2. CREATE & CONFIGURE QUORUM
---

Use minimum **50GB** Quorum Volume
Port attribute for Quorum is **Bidirectional, WIN_EX (2,7,22,25,40)**

##### 1. Change the attribute of ports to Bidirectional (//: Do not run for E1090)
	// raidcom modify port -port CL1-D -port_attribute ALL -IH0
	// raidcom modify port -port CL8-D -port_attribute ALL -IH0
	// raidcom modify port -port CL1-D -port_attribute ALL -IH1
	// raidcom modify port -port CL8-D -port_attribute ALL -IH1

Display port information

	raidcom get port -IH0
	raidcom get port -IH1

##### 2. Discover external storage, display and map LU
Path group id is 1, external group number is 1-1

	raidcom discover external_storage -port CL1-D -IH0
	raidcom discover lun -port CL1-D -external_wwn 257000c0fff1f0c4 -IH0
	raidcom add external_grp -path_grp 1 -external_grp_id 1-1 -port CL1-D -external_wwn 257000c0fff1f0c4 -lun_id 0 -IH0
	raidcom discover external_storage -port CL8-D -IH0
	raidcom discover lun -port CL8-D -external_wwn 277000c0fff1f0c4 -IH0
	raidcom add path -path_grp 1 -port CL8-D -external_wwn 277000c0fff1f0c4 -IH0

Path group id is 1, external group number is 2-1

	raidcom discover external_storage -port CL1-D -IH1
	raidcom discover lun -port CL1-D -external_wwn 257000c0fff1f0c4 -IH1
	raidcom add external_grp -path_grp 1 -external_grp_id 2-1 -port CL1-D -external_wwn 257000c0fff1f0c4 -lun_id 0 -IH1
	raidcom discover external_storage -port CL8-D -IH1
	raidcom discover lun -port CL8-D -external_wwn 277000c0fff1f0c4 -IH1
	raidcom add path -path_grp 1 -port CL8-D -external_wwn 277000c0fff1f0c4 -IH1

Confirm that asynchronous command processing has completed

	raidcom get command_status -IH0
	raidcom get command_status -IH1

Display external path information

	raidcom get path -path_grp 1 -IH0
	raidcom get path -path_grp 1 -IH1

##### 3. Create external volumes
	raidcom add ldev -external_grp_id 1-1 -ldev_id 0x3F00 -capacity all -IH0
	raidcom add ldev -external_grp_id 1-2 -ldev_id 0x3F00 -capacity all -IH1
	raidcom modify ldev -ldev_id 0x3F00 -ldev_name GAD_QUORUM -IH0
	raidcom modify ldev -ldev_id 0x3F00 -ldev_name GAD_QUORUM -IH1

Confirm that asynchronous command processing has completed

	raidcom get command_status -IH0
	raidcom get command_status -IH1

Display information about the volume

	raidcom get ldev -ldev_id 0x3F00 -fx -IH0
	raidcom get ldev -ldev_id 0x3F00 -fx -IH1

##### 4. Set external volume as quorum disk
	raidcom modify ldev -ldev_id 0x3F00 -quorum_enable 715062 M800 -quorum_id 0 -IH0
	raidcom modify ldev -ldev_id 0x3F00 -quorum_enable 745175 M800 -quorum_id 0 -IH1

Confirm that asynchronous command processing has completed

	raidcom get command_status -IH0
	raidcom get command_status -IH1

Display information about the volume

	raidcom get ldev -ldev_id 0x3F00 -fx -IH0
	raidcom get ldev -ldev_id 0x3F00 -fx -IH1



#### 3. CREATE & CONFIGURE VSM
---

##### 1. Create VSM
	raidcom add resource -resource_name NVMEGAD -virtual_type 715996 RH10MHF -IH0
	raidcom add resource -resource_name NVMEGAD -virtual_type 715996 RH10MHF -IH1

Display resources

	raidcom get resource -key opt -IH0
	raidcom get resource -key opt -IH1

##### 2. Add Host Groups to VSM
```bash
VSMPORTS=”CL1-A
CL3-A
CL5-A
CL7-A
CL3-B
CL5-B
CL7-B
CL3-D
CL5-D
CL2-A
CL4-A
CL6-A
CL8-A
CL2-B
CL4-B
CL8-B
CL2-D
CL6-D”
while read PORT; do
	for i in $(seq 128 240); do
		raidcom add resource -resource_name NVMEGAD -port ${PORT}-${i} -IH0
		raidcom add resource -resource_name NVMEGAD -port ${PORT}-${i} -IH1
	done
done < <(echo "${VSMPORTS}")

```
Check Host Groups

	raidcom get host_grp -port CL1-A -key host_grp -resource 1 -IH0
	raidcom get host_grp -port CL1-A -key host_grp -resource 1 -IH1

##### 3. Add LDEV IDs to VSM
```bash
VSLD=0x0000
VELD=512
VSLD=$(echo $((${VSLD})))
VELD=$(expr ${VSLD} \+ ${VELD} \-1)
for i in $(seq ${VSLD} ${VELD}); do
	raidcom unmap resource -ldev_id ${i} -virtual_ldev_id ${i} -IH0
	raidcom add resource -resource_name ${VNAM} -ldev_id ${i} -IH0
	raidcom map resource -ldev_id ${i} -virtual_ldev_id ${i} -IH0
	raidcom unmap resource -ldev_id ${i} -virtual_ldev_id ${i} -IH1
	raidcom add resource -resource_name ${VNAM} -ldev_id ${i} -IH1
	raidcom map resource -ldev_id ${i} -virtual_ldev_id reserve -IH1
done
```
Check LDEVs

	raidcom get ldev -ldev_id ${VSLD} -fx -IH0
	raidcom get ldev -ldev_id ${VSLD} -fx -IH1



#### 4. CREATE & CONFIGURE HOST GROUPS
---

##### First Host
	raidcom add host_grp -port CL6-A-128 -host_grp_name pvsintps21 -IH0
	raidcom modify host_grp -port CL6-A-128 -host_mode VMWARE_EX -set_host_mode_opt 2 22 25 40 54 63 68 110 -IH0
	raidcom add hba_wwn -port CL6-A-128 -hba_wwn 51402ec0172ccda0 -IH0
	raidcom set hba_wwn -port CL6-A-128 -hba_wwn 51402ec0172ccda0 -wwn_nickname pvsintps21_P1_0 -IH0
	raidcom add host_grp -port CL3-D-128 -host_grp_name pvsintps21 -IH0
	raidcom modify host_grp -port CL3-D-128 -host_mode VMWARE_EX -set_host_mode_opt 2 22 25 40 54 63 68 110 -IH0
	raidcom add hba_wwn -port CL3-D-128 -hba_wwn 51402ec0172cc9a8 -IH0
	raidcom set hba_wwn -port CL3-D-128 -hba_wwn 51402ec0172cc9a8 -wwn_nickname pvsintps21_P0_0 -IH0
	raidcom add host_grp -port CL6-A-128 -host_grp_name pvsintps21 -IH1
	raidcom modify host_grp -port CL6-A-128 -host_mode VMWARE_EX -set_host_mode_opt 2 22 25 40 54 63 68 110 -IH1
	raidcom add hba_wwn -port CL6-A-128 -hba_wwn 51402ec0172ccda0 -IH1
	raidcom set hba_wwn -port CL6-A-128 -hba_wwn 51402ec0172ccda0 -wwn_nickname pvsintps21_P1_0 -IH1
	raidcom add host_grp -port CL3-D-128 -host_grp_name pvsintps21 -IH1
	raidcom modify host_grp -port CL3-D-128 -host_mode VMWARE_EX -set_host_mode_opt 2 22 25 40 54 63 68 110 -IH1
	raidcom add hba_wwn -port CL3-D-128 -hba_wwn 51402ec0172cc9a8 -IH1
	raidcom set hba_wwn -port CL3-D-128 -hba_wwn 51402ec0172cc9a8 -wwn_nickname pvsintps21_P0_0 -IH1

##### Second Host
	raidcom add host_grp -port CL3-A-128 -host_grp_name pvsintps22 -IH0
	raidcom modify host_grp -port CL3-A-128 -host_mode VMWARE_EX -set_host_mode_opt 2 22 25 40 54 63 68 110 -IH0
	raidcom add hba_wwn -port CL3-A-128 -hba_wwn 51402ec0172cbd74 -IH0
	raidcom set hba_wwn -port CL3-A-128 -hba_wwn 51402ec0172cbd74 -wwn_nickname pvsintps22_P0_0 -IH0
	raidcom add host_grp -port CL6-D-128 -host_grp_name pvsintps22 -IH0
	raidcom modify host_grp -port CL6-D-128 -host_mode VMWARE_EX -set_host_mode_opt 2 22 25 40 54 63 68 110 -IH0
	raidcom add hba_wwn -port CL6-D-128 -hba_wwn 51402ec0172cbd7c -IH0
	raidcom set hba_wwn -port CL6-D-128 -hba_wwn 51402ec0172cbd7c -wwn_nickname pvsintps22_P1_0 -IH0
	raidcom add host_grp -port CL3-A-128 -host_grp_name pvsintps22 -IH1
	raidcom modify host_grp -port CL3-A-128 -host_mode VMWARE_EX -set_host_mode_opt 2 22 25 40 54 63 68 110 -IH1
	raidcom add hba_wwn -port CL3-A-128 -hba_wwn 51402ec0172cbd74 -IH1
	raidcom set hba_wwn -port CL3-A-128 -hba_wwn 51402ec0172cbd74 -wwn_nickname pvsintps22_P0_0 -IH1
	raidcom add host_grp -port CL6-D-128 -host_grp_name pvsintps22 -IH1
	raidcom modify host_grp -port CL6-D-128 -host_mode VMWARE_EX -set_host_mode_opt 2 22 25 40 54 63 68 110 -IH1
	raidcom add hba_wwn -port CL6-D-128 -hba_wwn 51402ec0172cbd7c -IH1
	raidcom set hba_wwn -port CL6-D-128 -hba_wwn 51402ec0172cbd7c -wwn_nickname pvsintps22_P1_0 -IH1



#### 5. CREATE & ASSIGN VOLUMES
---

##### 1. Create Volumes
	raidcom add ldev -pool 0 -ldev_id 0 -capacity 4T -capacity_saving deduplication_compression -IH0
	raidcom add ldev -pool 0 -ldev_id 1 -capacity 4T -capacity_saving deduplication_compression -IH0
	raidcom add ldev -pool 0 -ldev_id 0 -capacity 4T -capacity_saving deduplication_compression -IH1
	raidcom add ldev -pool 0 -ldev_id 1 -capacity 4T -capacity_saving deduplication_compression -IH1
	raidcom modify ldev -ldev_id 0 -ldev_name BVMINT02_POOL01 -IH0
	raidcom modify ldev -ldev_id 1 -ldev_name BVMINT02_POOL02 -IH0
	raidcom modify ldev -ldev_id 0 -ldev_name BVMINT02_POOL01 -IH1
	raidcom modify ldev -ldev_id 1 -ldev_name BVMINT02_POOL02 -IH1
	raidcom get ldev -ldev_id 0 -fx -IH0
	raidcom get ldev -ldev_id 1 -fx -IH0
	raidcom get ldev -ldev_id 0 -fx -IH1
	raidcom get ldev -ldev_id 1 -fx -IH1

##### 2. Add LUNs
	raidcom add lun -port CL6-A-128 -lun_id 0 -ldev_id 0 -IH0
	raidcom add lun -port CL6-A-128 -lun_id 1 -ldev_id 1 -IH0
	raidcom get lun -port CL6-A-128 -fx -IH0
	raidcom add lun -port CL3-D-128 -lun_id 0 -ldev_id 0 -IH0
	raidcom add lun -port CL3-D-128 -lun_id 1 -ldev_id 1 -IH0
	raidcom get lun -port CL3-D-128 -fx -IH0
	raidcom add lun -port CL3-A-128 -lun_id 0 -ldev_id 0 -IH0
	raidcom add lun -port CL3-A-128 -lun_id 1 -ldev_id 1 -IH0
	raidcom get lun -port CL3-A-128 -fx -IH0
	raidcom add lun -port CL6-D-128 -lun_id 0 -ldev_id 0 -IH0
	raidcom add lun -port CL6-D-128 -lun_id 1 -ldev_id 1 -IH0
	raidcom get lun -port CL6-D-128 -fx -IH0
	
	raidcom add lun -port CL6-A-128 -lun_id 0 -ldev_id 0 -IH1
	raidcom add lun -port CL6-A-128 -lun_id 1 -ldev_id 1 -IH1
	raidcom get lun -port CL6-A-128 -fx -IH1
	raidcom add lun -port CL3-D-128 -lun_id 0 -ldev_id 0 -IH1
	raidcom add lun -port CL3-D-128 -lun_id 1 -ldev_id 1 -IH1
	raidcom get lun -port CL3-D-128 -fx -IH1
	raidcom add lun -port CL3-A-128 -lun_id 0 -ldev_id 0 -IH1
	raidcom add lun -port CL3-A-128 -lun_id 1 -ldev_id 1 -IH1
	raidcom get lun -port CL3-A-128 -fx -IH0
	raidcom add lun -port CL6-D-128 -lun_id 0 -ldev_id 0 -IH1
	raidcom add lun -port CL6-D-128 -lun_id 1 -ldev_id 1 -IH1
	raidcom get lun -port CL6-D-128 -fx -IH0



#### 6. RECONFIGURE HORCM FILES for GAD
---

##### 1. Stop Instances
	horcmshutdown 0 1

##### 2. Reconfigure and Start Instances
	horcmstart 0 1
	raidcom -login horcm Passw00rd -IH0
	raidcom -login horcm Passw00rd -IH1



#### 7. CONTROL VIRTUAL LDEV ID on SECONDARY SITE
---

##### 1. Verify the virtual LDEV ID at the secondary site
	raidcom get ldev -ldev_id 0 -fx -IH100
	raidcom get ldev -ldev_id 1 -fx -IH100

##### 2. Check that the same virtual LDEV ID as that of the primary volume does not exist in the virtual storage machine of the secondary site. 
	raidcom get ldev -ldev_id 0 -key front_end -cnt 1 -fx -IH101
	raidcom get ldev -ldev_id 1 -key front_end -cnt 1 -fx -IH101

##### 3. Confirm the actual LDEV ID of the volume whose virtual LDEV ID is 0
	raidcom get ldev -ldev_id 0 -fx -IH101
	raidcom get ldev -ldev_id 1 -fx -IH101

##### 4. Remove assignment of the virtual LDEV ID 0 from the volume whose LDEV ID is <<PHY_LDEV>>
	raidcom unmap resource -ldev_id <<PHY_LDEV>> -virtual_ldev_id 0 -IH1
	raidcom get ldev -ldev_id 0 -key front_end -cnt 1 -fx -IH101



#### 8. SET ALUA MODE
---

	raidcom modify ldev -ldev_id 0 -alua enable -IH0
	raidcom get ldev -ldev_id 0 -fx -IH0



#### 9. CREATE GAD PAIRS
---

	paircreate -g bvmint02 -fg never 1 -vl -jq 0 -IH0
	pairdisplay -g bvmint02 -fxce -IH0



#### 10. SET PREFERRED, non PREFERRED PATHS
---

	raidcom modify lun -port CL6-A-128 -lun_id all -asymmetric_access_state optimized -IH0
	raidcom get lun -port CL6-A-128 -key opt_page1 -fx -IH0
	raidcom modify lun -port CL3-D-128 -lun_id all -asymmetric_access_state optimized -IH0
	raidcom get lun -port CL3-D-128 -key opt_page1 -fx -IH0
	raidcom modify lun -port CL6-A-128 -lun_id all -asymmetric_access_state non_optimized -IH1
	raidcom get lun -port CL6-A-128 -key opt_page1 -fx -IH1
	raidcom modify lun -port CL3-D-128 -lun_id all -asymmetric_access_state non_optimized -IH1
	raidcom get lun -port CL3-D-128 -key opt_page1 -fx -IH1
