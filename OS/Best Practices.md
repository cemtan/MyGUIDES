### LINUX
---

##### max_sectors_kb = 512 https://access.redhat.com/solutions/43861
---

	multipath.conf → max_sectors_kb
	echo → echo 128 > /sys/block/sda/queue/max_sectors_kb
	udev → ACTION=="add|change", SUBSYSTEM=="block", ENV{ID_VENDOR}=="3PARdata", ENV{ID_MODEL}=="VV", ATTR{queue/max_sectors_kb}+="128"

##### nomerges = 0
---

	udev → ACTION=="add|change", KERNEL=="sd*[!0-9]", ATTR{queue/nomerges}="0"




### VMWARE
---

https://docs.hitachivantara.com/v/u/en-us/application-optimized-solutions/mk-sl-145