### Setup
---
---



	systemctl set-default multi-user.target
	systemctl isolate  multi-user.target
	systemctl stop firewalld
	systemctl disable firewalld
	