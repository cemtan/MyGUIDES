#### DEDUP - INLINE TO POST PROCESS
---
---


```bash
#!/bin/bash

ldevs=$(raidcom get ldev -ldev_list mapped -I${1} | grep -e "^LDEV :" -e CSV_PROCESS_MODE | awk '{printf("%s%s",$0,NR%2?",":"\n")}' | grep -i INLINE | cut -d, -f1 | cut -d' ' -f3)

for ldev in ${ldevs}; do
	raidcom modify ldev -ldev_id ${ldev} -capacity_saving_mode post_process -I${1}
done

result=$(raidcom get ldev -ldev_list mapped -I${1} | grep -e "^LDEV :" -e CSV_PROCESS_MODE | awk '{printf("%s%s",$0,NR%2?",":"\n")}')
icount=$(echo "${result}" | grep -i INLINE | wc -l)
pcount=$(echo "${result}" | grep -i POST_PROCESS | wc -l)
echo "INLINE COUNT: ${icount}"
echo "POST_PROCESS COUNT: ${pcount}"
```
