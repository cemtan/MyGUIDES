#### CHECK SIGNAL
---
---


```bash
#!/bin/bash

source helpers.sh
out=${outdir}"brocade_checksignal_$(date +%F).txt"

# INITIALIZE THE FILE
date +%F_%T > ${out}
echo " " >> ${out}
echo "------------------------------------------------------------------------------------------" >> ${out}
echo "                   ####  Brocade Switch Check Signal Levels  ####" >> ${out}
echo "------------------------------------------------------------------------------------------" >> ${out}

# ITERATIONS
for switch in ${switches}; do
	echo "# SWITCH: "${switch} >> ${out}
	switchshow=$(runcmd ${buser}@${switch} switchshow)
	echo "${switchshow}" | grep "switchName:" >> ${out}
	portslot=$(echo "${switchshow}" | grep -e Online -e No_Light |grep -v "switchState:"|grep -vi disabled |awk -F " " '{print $2"/"$3}')
	for posl in ${portslot}; do
		sfpshow=$(runcmd ${buser}@${switch} sfpshow ${posl} -f)
		tx=$(echo "${sfpshow}" | grep -i "TX Power" | awk -F " " '{print $5}'| sed -e 's/(g' -e 's/)g' -e 's/uW//g'| awk '{printf "%d\n",$1}')
		rx=$(echo "${sfpshow}" | grep -i "RX Power" | awk -F " " '{print $5}'| sed -e 's/(g' -e 's/)g' -e 's/uW//g'| awk '{printf "%d\n",$1}')
		[ ${tx} -lt 260 ] && echo $posl" TX Power: " ${tx}
		[ ${rx} -lt 120 ] && echo $posl" RX Power: " ${rx}
	done
	echo "------------------------------------------------------------------------------------------" >> ${out}
done

```
