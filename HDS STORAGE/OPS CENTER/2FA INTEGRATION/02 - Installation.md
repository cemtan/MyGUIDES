## Installation
---
---



### 1. Hitachi OPS Center
---
---

Deployed from OVA



### 2. Keycloak 1 and 2 (merubuntu22, merubuntu24)
---
---

Install steps are same for both ubuntu server except hostname.
You can install only one if you plan to use only SAML or OpenID.


#### 2.1 Install OS
---

Install Ubuntu server including GUI (GUI optional)
Configured hostname, ip,DNS according to previous table
Although DNS ise used, ip addresses of all servers are written in hosts file of each server.
For ubuntu and OPS Center /etc/hosts file
For windows c:\windows\system32\drives\etc\hosts


#### 2.2 Keycloak Install
---

- Login ubuntu with ssh (sample configured user mer for this document)
- Install OpenJDK 17 or later
```bash
	sudo apt install openjdk-17-jdk
```
- Download keycloak
  If server has no access to internet just download it from another PC which has internet access and copy to server: https://www.keycloak.org/downloads
```bash
	wget https://github.com/keycloak/keycloak/releases/download/26.3.0/keycloak-26.3.0.tar.gz
```
- Extract downloaded keycloak file
```bash
	tar -xzf keycloak-26.3.0.tar.gz
```
- Move keycloak folder to /opt ( not mandatory but keep running programs in user folder not recommended)
```bash
	sudo mv keycloak-26.3.0 /opt/keycloak
	cd /opt/keycloak/
	ls -l
	cd bin
```
- Create self-singed certificate.
	- First Command: If customer has official CA certified certificate it must be imported to ubuntu. Please check OS documents for importing CA certificate
	- Second command: Replace merubuntu22 with your ubuntu hostname.
	- Third Command: It will ask export password. For this document 123 is used as password.
```bash
	openssl genpkey -algorithm RSA -out keycloak.local.key -pkeyopt rsa_keygen_bits:2048
	openssl req -new -x509 -sha256 -key keycloak.local.key -out keycloak.local.crt -days 720 -subj "/C=TR/ST=Istanbul/L=Istanbul/O=MERORG/CN=merubuntu22"
	openssl pkcs12 -export -in keycloak.local.crt -inkey keycloak.local.key -name keycloak -out keycloak.local.p12
```
- **OPTIONAL** - Configure Database for keycloak
	Keycloak normally runs with config files under keycloak folder.But for production environment using a database recommended.
	If you decide to use DB for keycloak these are the steps to prepare PostgreSQL DB for keycloak.
```bash
	sudo apt install postgresql postgresql-contrib
	sudo -i -u postgres
	psql
	CREATE DATABASE keycloak_db;
	CREATE USER mer_keycloak_user WITH ENCRYPTED PASSWORD 'Passw00rd';
	ALTER USER mer_keycloak_user WITH PASSWORD 'Passw00rd';
	GRANT ALL PRIVILEGES ON DATABASE keycloak_db TO mer_keycloak_user;
```


#### 2.3 Running Keycloak
---
```bash
	export KC_BOOTSTRAP_ADMIN_USERNAME=admin
	export KC_BOOTSTRAP_ADMIN_PASSWORD=Passw00rd
```

##### 2.3.1 If postgresql not used;
   Change merubuntu22 with your hostname.
   Keystore password which is used while creating self-signed certificate.
   Port can be changed if requested.
```bash
	sudo ./kc.sh start \
	--https-port=8443 \
	--https-key-store-file=keycloak.local.p12 \
	--https-key-store-password="123" \
	--https-key-store-type=pkcs12 \
	--hostname=merubuntu22
```

##### 2.3.1 If postgresql used;
   Change merubuntu22 with your hostname.
   Keystore password which is used while creating self-signed certificate.
   Port can be changed if requested
```bash
	sudo ./kc.sh start \
	--db=postgres \
	--db-url=jdbc:postgresql://localhost/keycloak_db \
	--db-username=mer_keycloak_user \
	--db-password="Passw00rd" \
	--https-port=8443 \
	--https-key-store-file=keycloak.local.p12 \
	--https-key-store-password="123" \
	--https-key-store-type=pkcs12 \
	--hostname=merubuntu22
```
sudo ./kc.sh start \
--db=postgres \
--db-url=jdbc:postgresql://localhost/keycloak_db \
--db-username=mer_keycloak_user \
--db-password="Passw00rd" \
--https-port=8443 \
--https-key-store-file=keycloak.local.p12 \
--https-key-store-password="123" \
--https-key-store-type=pkcs12 \
--hostname=merubuntu22