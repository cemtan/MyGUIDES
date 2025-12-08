#### Virtual Fabric Commands
---
---



Connect to the physical chassis and log in using an account with the chassis-role permission.

**Show Virtual Fabric Setup**
```bash
fosconfig --show
```

**Enable Virtual Fabric**
```bash
fosconfig --enable vf
```

**Disable Virtual Fabric**
```bash
fosconfig --disablevf
```

**Move port(s) to the default logical switch**
```bash
lscfg --config 128 -slot <slot> -port <port>
```

**Delete all the non-default logical switch**
```bash
lscfg --delete <fabricID>
```


https://techdocs.broadcom.com/us/en/fibre-channel-networking/fabric-os/fabric-os-administration/9-1-x/v26769804/v26769683.html 
