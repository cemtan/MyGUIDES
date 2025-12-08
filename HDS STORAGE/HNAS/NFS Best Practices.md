#### NFS BEST PRACTICES
---
---



##### Needed PORTS
---

	111: TCP, UDP, port mapper
	2049: TCP, UDP, NFS
	1110: TCP, UDP, cluster (not needed for HNAS)
	4045: TCP, UDP, NFS lock manager


	
##### Maximum Supported Version of NFS Protocol
---

	    nfs-max-supported-version 4.1



##### Security Mode
---

The security-mode command is used to set the file system security mode for the current EVS security context, a file system or a virtual volume.

	security-mode set -f  <FS> mixed


 
##### Set Ownership (especially if Security Mode is UNIX)
---

Configures how the file system modifies a DACL when an NFS client applies a Unix mode or ownership change to a file/directory that has a security descriptor.

	fs-dacl-mode mask-on-chmod-or-create

	where
	mask-on-chmod-or-create 
			Permissions granted by the security descriptor's DACL to the associated file system object, which are not granted by the mode, are "masked", such that:
		
			Permissions granted to the owner are no greater than those granted by the mode's user bits.
			Permissions granted to other users/groups are no greater than those granted by the mode's group bits.
			ACEs are also amended and/or appended to the DACL, to reflect the permissions granted by the mode.
		
			See the --masking-deny-aces option for a description of how the masking is performed.
		
			N.B. Inheritable permissions are not affected.



##### Limit UDP Usage if it is asked
---

Enable/disable the registration of NFS over UDP with portmapper to control use of the service

	rpc-service-nfs-udp --disable



##### Set rsize / wsize
---

Configures the maximum number of bytes per NFSv3 READ request that a client can receive. (TCP & UDP)

	nfsv3-rsize-set
	
Configures the maximum number of bytes per NFSv3 WRITE request that a client can send whilst writing data to a file. (TCP & UDP)

	nfsv3-wsize-set
	


##### Set max-dir-size for excessive number of files
---

	max-dir-size --total-chunks 0x3000000



##### Other Recommendations
---

Bunların yanısıra NFS özelinde şunları söyleyebilirim:

1. Bazı client'lar NFS server'i sature edebilirler. Bu durumda örneğin linux için şu kernel paramterelerinin değerleri client tarafında azaltılabilir.

		/proc/sys/net/core/rmem_default
		/proc/sys/net/core/rmem_max

2. NFS access Kerberos üzerinde secure hale getirilebilir.
3. NFS service load balancer arkasına konulabilir ya da en azından failover yapacak şekilde ayarlanabilir.
4. NFS monitoring ve hatta gerekiyorsa auditing açılabilir.
