#### CHECK MAPS
---
---


```bash
#!/bin/bash

source helpers.sh
out=${outdir}"brocade_checkmaps_$(date +%F).txt"

# INITIALIZE THE FILE
date +%F_%T > ${out}
echo " " >> ${out}
echo "------------------------------------------------------------------------------------------" >> ${out}
echo "                   ####  Brocade Switch Check Maps  ####" >> ${out}
echo "------------------------------------------------------------------------------------------" >> ${out}

# ITERATIONS
for switch in ${switches}; do
	echo "# SWITCH: "${switch} >> ${out}
	switchshow=$(runcmd ${buser}@${switch} switchshow)
	echo "${switchshow}" | grep "switchName:" >> ${out}
	mapsout=$(runcmd ${buser}@${switch} mapsdb --show -category Port,FRU,Switch)
	echo "${mapsout}" >> ${out}
	echo "------------------------------------------------------------------------------------------" >> ${out}
done

```
