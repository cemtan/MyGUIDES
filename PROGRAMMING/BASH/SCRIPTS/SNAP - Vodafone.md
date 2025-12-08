#### SNAPSHOT - VODAFONE
---
---


```bash
#!/bin/bash

#Snap session da bahsettiğim başka bir yer için hazırladığım snap işlemleri ekte.
#Bu scripte en başta snap olduğu ve yenileme zamanı çalışan şekilde.
#İlk kez çalıştırıken baştaki unmap ve delete snap komutları atlanmalı henüz snap olmadığı için. Bir kez alındıktan sonra tüm script data yenileme için yeni snapshot olarak kullanılabilir.

date1=$(/bin/date +%Y.%m.%d.%H_%M_snap)
date2=$(/bin/date +%Y.%m.%d.%H_%M_snap_error)

echo "Script baslama zamani: $(date +%H:%M)" 1>/admin/$date1 2>/admin/$date2
horcmstart 1 1>>/admin/$date1 2>>/admin/$date2
raidcom -login snapuser snapg800 -I1

echo "Storage Resource lock: $(date +%H:%M)" 1>>/admin/$date1 2>>/admin/$date2
raidcom lock resource -resource_name meta_resource -I1 1>>/admin/$date1 2>>/admin/$date2

echo "Mevcut snaphotlar: $(date +%H:%M)" 1>>/admin/$date1 2>>/admin/$date2
raidcom get snapshot -snapshotgroup BNKPRE02 -key opt -format_time -I1 2>> /admin/$date1

echo "Mevcut Snapshot unmount zamani: $(date +%H:%M)" 1>>/admin/$date1 2>>/admin/$date2

raidcom unmap snapshot -ldev_id 00:6a -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:6b -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:6c -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:6d -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:6e -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:6f -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:70 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:71 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:72 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:73 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:74 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:75 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:76 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:77 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:78 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:79 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom unmap snapshot -ldev_id 00:90 -snapshotgroup BNKPRE02 -I1 1>>/admin/$date1 2>>/admin/$date2

sleep 60
echo "Mevcut Snapshot silme zamani: $(date +%H:%M)" 1>>/admin/$date1 2>>/admin/$date2
raidcom delete snapshot -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2

sleep 120

echo "Silme sonrasi snapshot listesi zamani: $(date +%H:%M)" 1>>/admin/$date1 2>>/admin/$date2

raidcom get snapshot -snapshotgroup BNKPRE02 -key opt -format_time -I1 1>>/admin/$date1 2>>/admin/$date2

echo "Yeni snapshot alma zamani: $(date +%H:%M)" 1>>/admin/$date1 2>>/admin/$date2

raidcom add snapshot -ldev_id 00:6a -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:6b -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:6c -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:6d -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:6e -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:6f -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:70 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:71 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:72 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:73 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:74 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:75 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:76 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:77 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:78 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:79 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom add snapshot -ldev_id 00:90 -pool 0 -snapshotgroup BNKPRE02 -snap_mode CTG -I1 1>>/admin/$date1 2>>/admin/$date2

sleep 120

echo "Snapshot split zamani: $(date +%H:%M)" 1>>/admin/$date1 2>>/admin/$date2
raidcom modify snapshot -snapshotgroup BNKPRE02 -snapshot_data create -I1 1>>/admin/$date1 2>>/admin/$date2

sleep 120

echo "Yeni snapshot mount zamani: $(date +%H:%M)" 1>>/admin/$date1 2>>/admin/$date2

raidcom map snapshot -ldev_id 00:6a 00:7c -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:6b 00:7d -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:6c 00:7e -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:6d 00:7f -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:6e 00:80 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:6f 00:81 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:70 00:82 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:71 00:83 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:72 00:84 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:73 00:85 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:74 00:86 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:75 00:87 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:76 00:88 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:77 00:89 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:78 00:8a -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:79 00:8b -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2
raidcom map snapshot -ldev_id 00:90 00:91 -snapshotgroup BNKPRE02  -I1 1>>/admin/$date1 2>>/admin/$date2

sleep 90

echo "Yeni snapshot listesi: $(date +%H:%M)" 1>>/admin/$date1 2>>/admin/$date2

raidcom get snapshot -snapshotgroup BNKPRE02 -key opt -format_time -I1 1>>/admin/$date1 2>>/admin/$date2

echo "Storage Resource unlock: $(date +%H:%M)" 1>>/admin/$date1 2>>/admin/$date2
raidcom unlock resource -resource_name meta_resource -I1 1>>/admin/$date1 2>>/admin/$date2

```
