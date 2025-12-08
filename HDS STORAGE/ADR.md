#### ADR SOM
---



1191 -> change inline to post-process if cpu >0 %50
1248 -> change inline to post-process if cache write pendings >= %30
```
raidcom modify system_opt -system_option_mode system -mode_id 1191 -mode enable -Ixxx
raidcom modify system_opt -system_option_mode system -mode_id 1248 -mode enable -password <One Time Password> -Ixxx
```


#### **ADR MONITOR**
---
```text-x-trilium-auto
raidcom get pool 2>&1 > pool.txt
raidcom get pool -key opt 2>&1 > poolopt.txt
raidcom get pool -key efficiency 2>&1 > poolefficiency.txt
raidcom get pool -key total_saving 2>&1 > pooltotsave.txt
raidcom get pool -key software_saving 2>&1 > pooladr.txt
raidcom get pool –key fmc 2>&1 > poolfmc.txt
raidcom get ldev -ldev_list dp_volume -fx 2>&1 > ldevlist.txt
raidcom get ldev -ldev_list dp_volume -key tier -fx 2>&1 > ldevtier.txt
raidcom get ldev -ldev_list dp_volume -key software_saving -fx 2>&1 > ldevadr.txt
```

