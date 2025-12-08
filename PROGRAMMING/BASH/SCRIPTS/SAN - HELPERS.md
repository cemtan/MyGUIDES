#### HELPERS
---
---


```bash
#!/bin/bash
buser="reportuser"
bpass="password" #Leave empty for passwordless login
outdir="/var/www/html/"
switches="10.0.0.1
10.0.0.2
10.0.0.3"

# INITIALIZE SSH COMMAND
runcmd() {
	if [ -z "${bpass}" ]; then
		ssh $@
	else
		sshpass -p ${bpass} ssh -o StrictHostKeyChecking=no $@
	fi
}
```
