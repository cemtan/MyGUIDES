#### CHECK UP
---
---


```bash
#!/bin/bash

source helpers.sh
out=${outdir}"brocade_checkup_$(date +%F).txt"

# INITIALIZE THE FILE
date +%F_%T > ${out}
echo " " >> ${out}
echo "------------------------------------------------------------------------------------------" >> ${out}
echo "                   ####  Brocade Switch Checkup  ####" >> ${out}
echo "------------------------------------------------------------------------------------------" >> ${out}

# ITERATIONS
for switch in ${switches}; do
	echo "# SWITCH: "${switch} >> ${out}
	switchshow=$(runcmd ${buser}@${switch} switchshow)
	chassisshow=$(runcmd ${buser}@${switch} chassisshow)
	version=$(runcmd ${buser}@${switch} version)
	porterrshow=$(runcmd ${buser}@${switch} porterrshow)
	echo "${switchshow}" | grep "switchName:" >> ${out}
	echo "${version}" | grep "Fabric OS:" >> ${out}
	echo "${chassisshow}" | grep "Chassis Family:" >> ${out}
	echo "${chassisshow}" | grep "Chassis Factory Serial Num:" >> ${out}
	echo "Total FC Port: "$(echo "${switchshow}" | grep -v No_Module | grep id -wc) >> ${out}
	echo "Used FC Port: "$(echo "${switchshow}" | grep Online | grep -v State | grep -v grep -wc) >> ${out}
	echo "Unused FC Port: "$(echo "${switchshow}" | grep -ve Online -ve No_Module | grep id | grep -v grep -wc) >> ${out}
	if echo "${switchshow}" | grep -i laser >> /dev/null; then
		echo "Laser Fault SFPs: " >> ${out}
		echo "${switchshow}" | grep Index >> ${out}
		echo "${switchshow}" | grep -i laser
	else
		echo "No Laser Fault SFP is Found." >> ${out}
	fi
	if echo "${switchshow}" | grep -i exceeded >> /dev/null; then
		echo "Fenced Ports: " >> ${out}
		echo "${switchshow}" | grep Index >> ${out}
		echo "${switchshow}" | grep -i exceeded
	else
		echo "No Port Fence is Found." >> ${out}
	fi
	porterrcount=$(echo "${porterrshow}"|grep -v "0       0       0       0       0       0       0       0       0       0       0       0       0       0       0       0       0" -wc)
	if [ ${porterrcount} -gt 2 ]; then
		echo "Port Errors: " >> ${out}
		echo "${porterrshow}"|grep -v "0       0       0       0       0       0       0       0       0       0       0       0       0       0       0       0       0" >> ${out}
	else
		echo "No Port Error is Found." >> ${out}
	fi
	# Clear Stats
	runcmd ${buser}@${switch} statsclear
	echo "------------------------------------------------------------------------------------------" >> ${out}
done


```