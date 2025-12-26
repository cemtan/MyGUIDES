#### LOCAL REPOSITORY
---
---



**1. ISO indir**
https://yum.oracle.com/ISOS/OracleLinux/OL9/u4/x86_64/OracleLinux-R9-U4-x86_64-dvd.iso

**2. ISO dosyasını OPS Center root folder'a kopyala**
```bash
/root/OracleLinux-R9-U4-x86_64-dvd.iso
```
**3. ISO dosyasını mount et**
```bash
mount /root/OracleLinux-R9-U4-x86_64-dvd.iso /mnt
```
**4. Local repository oluştur**
```bash
cat <<EOT >> /etc/yum.repos.d/local.repo
[InstallMedia-BaseOS]
name=BaseOS
baseurl=file:///root/cd
gpgcheck=0
enabled=1
EOT
```
**5. Repository'i clean ve update et**
```bash
yum clean all
yum update
```
