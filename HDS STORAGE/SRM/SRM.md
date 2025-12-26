#### SRM
---
---



SRA link
https://support.hitachivantara.com/en/user/answers/downloads/downloads-detail.html?d=VMware%20Adapters&pptype=Hardware%20Components

CCI link
https://support.hitachivantara.com/en/user/answers/downloads/downloads-detail.html?d=Command%20Control%20Interface%20-%20Enterprise&pptype=Software%20Version

	Primary site horcm0.conf   		→  HUR p-vol & GAD P-vol
	Primary site horcm3.conf   		→  GAD S-vol  & hur p-vol-delta
	remode site horcm1.conf    		→  HUR s-vol & hurDelta s-vol & Snapshot p-vol
	remote site horcm2.conf    		→  Snapshot s-vol

→ DR ortamında snapshot s-vol esx sunuculara verilmiş olacak
 
Production site SRM ve CCI  server
DR site SRM ve CCI server

→ vmware sra ya root olarak login oldu. aynı işlemler DR için de yapılacak

	cd /opt/vmware/support/logs/srm/SRAs/sha256*
	scp rmsra20.linux64 ---> CCI sunucusu

→ CCI sunucusuna root olarak login olup

	cp -p /HORCM/usr/bin/rmsra20  /HORCM/usr/bin/rmsra20.backup
	cp {srm den copyalana rmsra20.linux64} /HORCM/usr/bin/rmsra20
	ls -al /HORCM/usr/var
	export HORCC_AUTH_UID=HTSRA
	raidcom -logout -IH0
	raidqry -g IH0
	ls -al /HORCM/usr/var

→ vmware sra ya root olarak login oldu.
  
	cd /var/lib/docker/volumes/c2*_v
	vi env.conf
					RMSRATOV=60
					RMSTATMU=0
					RMSRAFNGPTCHK=yes
	chmod a+r env.conf

	docker run -i -d --name get_fngpnt self/sra:RMCMD
	docker exec -it get_fngpnt bash
	ssh-keyscan -t sra SRA_IP > known_hosts_file
	cat known_hosts_file ****copy to notepad
	exit
	docker stop get_fngpnt
	docker rm  get_fngpnt
	vi /var/lib/docker/volumes/c2*_v/_data/known_hosts <<<--- ssh key paste
	chmod a+r var/lib/docker/volumes/c2*_v/_data/known_hosts

→ Snapshot s-vol esx sunuculara verilmiş olacak
