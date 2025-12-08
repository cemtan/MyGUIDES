#### OPS Center - SSL config with cssslsetup tool 2
---
---



Administrator, protector, automator,conf Manager installed case

**1. Step**
```bash
cd /opt/hitachi/CommonService/utility/bin 
./cssslsetup 
```
		
		 Main menu    Ver:10.9.1-00 
		 1. Create certificate signing request and private key. 
		 2. Set up SSL server. 
		 3. Set up SSL client. 
		 4. Enable/disable certificate verification(optional). 
		 5. Restart services for each product. 
		 Enter a number or q to quit: 1 
		 Please enter the following items. 
		 Enter the absolute path to the private key [default=/var/opt/hitachi/opscenter/ssl/server/httpsdkey.pem]: ENTER
		 Enter the absolute path to certificate signing request [default=/var/opt/hitachi/opscenter/ssl/server/httpsd.csr]: ENTER
		 Specifies the signature algorithm for the server certificate for RSA cryptography. 
		 1. SHA-256 
		 2. SHA-512 
		 Enter a number [default=1]: 1
		 Select the key size. 
		 1. 2048 
		 2. 3072 
		 3. 4096 
		 Enter a number [default=1]: 1
		 Enter server name (CN) [default=merops]: gbopcp201
		 Enter organizational unit (OU): PS 
		 Enter organization name (O) [default=merops]: MER 
		 Enter your city or locality (L): Istanbul 
		 Enter your state or province (ST): NA 
		 Enter your two-character country-code (C): TR 
		 Enter host name(or FQDN), IP address or both of SubjectAltName. 
		 -Host name(or FQDN) of SubjectAltName: gbopcp201
		 -IP address of SubjectAltName: 192.168.80.46 
		 Your settings are complete: 
		 
		 Absolute path to the private key : /var/opt/hitachi/opscenter/ssl/server/httpsdkey.pem 
		 Absolute path to certificate signing request : /var/opt/hitachi/opscenter/ssl/server/httpsd.csr 
		 Signature algorithm for the server certificate for RSA cryptography [default=1] : 1. SHA-256 
		 Key size [default=1] : 1. 2048 
		 CN=merops 
		 OU=PS 
		 O=MER 
		 L=Istanbul 
		 ST=NA 
		 C=TR 
		 SAN(Host name(or FQDN)=merops , IP address=192.168.80.46 ) 
		 ----------------------------------------------------------------
		 Are all the above settings correct? 
		 1. Yes 
		 2. No (Cancel) 
		 Enter a number [default=1]: 1
		 Creating certificate signing request and private key. 
		 KAOP64001-I Signing request and private key created successfully. 
		 ----------------------------------------------------------------
		 Certificate Request: 
			 Data: 
				 Version: 1 (0x0) 
				 Subject: C = TR, ST = NA, L = Istanbul, O = MER, OU = PS, CN = merops 
				 Subject Public Key Info: 
					 Public Key Algorithm: rsaEncryption 
						 RSA Public-Key: (2048 bit) 
						 Modulus: 
							 00:ad:d4:c0:cd:34:31:b5:c4:85:14:a8:43:ce:b6: 
							 dd:ca:65:2d:7d:97:6a:a7:37:33:26:7d:d1:7f:39: 
							 a7:d9:15:50:9c:8c:fd:d9:07:9b:e7:15:1c:fd:30: 
							 41:e6:b8:b9:e0:13:62:f9:66:bb:12:de:69:9c:24: 
							 d6:cc:7a:53:01:7b:7e:c7:24:5c:9f:e4:a2:ed:dd: 
							 59:9c:d3:6a:5b:76:81:47:ad:9a:1f:20:d3:7b:a7: 
							 88:ef:59:8c:1a:74:46:b9:ee:91:7d:e5:30:42:5f: 
							 47:51:24:77:57:50:6f:aa:07:e3:4e:70:33:7c:46: 
							 b5:0a:76:7f:1c:46:81:98:91:d9:6f:9a:07:eb:ed: 
							 35:6e:41:8d:90:01:bb:ce:d5:96:ba:e4:b6:d4:b6: 
							 6b:4d:a9:0d:c4:e3:ef:28:84:c5:51:15:af:39:5a: 
							 8b:02:27:0d:66:fc:90:57:cc:a7:6e:d4:54:d7:dc: 
							 a5:a4:2b:00:b1:b8:1e:fb:8d:c7:a8:de:9d:c7:cd: 
							 24:6d:f5:a5:6b:bb:47:1c:45:18:ff:82:ad:75:2f: 
							 59:a5:e4:82:c2:8a:7b:d2:4d:ff:cd:cf:32:93:c5: 
							 0a:6f:4e:f7:3b:f5:36:ed:0d:bb:89:93:e1:9b:e4: 
							 c0:c8:42:93:13:e4:5a:0e:6f:cc:18:f9:ed:3f:7c: 
							 85:4b 
						 Exponent: 65537 (0x10001) 
				 Attributes: 
				 Requested Extensions: 
					 X509v3 Subject Alternative Name: 
						 DNS:merops, IP Address:192.168.80.46 
					 X509v3 Basic Constraints: 
						 CA:FALSE 
					 X509v3 Key Usage: 
						 Digital Signature, Non Repudiation, Key Encipherment 
					 X509v3 Extended Key Usage: 
						 TLS Web Server Authentication, TLS Web Client Authentication 
			 Signature Algorithm: sha256WithRSAEncryption 
				  1d:a1:44:ac:c4:f6:2b:b8:31:7e:a1:ae:0d:fc:cf:c9:2b:3a: 
				  3e:2e:cb:40:5a:34:97:e8:de:24:f2:18:37:89:85:32:1a:74: 
				  48:26:79:84:ef:b1:9e:de:85:a3:78:dd:20:d2:c8:87:1e:2c: 
				  8b:f6:49:01:27:70:6e:43:16:a7:d1:69:af:06:72:c9:b7:bb: 
				  e2:79:7d:d2:b2:1d:7a:58:74:af:45:eb:aa:4f:63:63:01:1c: 
				  2d:d6:70:13:32:5c:ae:e2:ba:3a:42:65:e9:29:9b:1b:03:45: 
				  42:30:56:62:91:91:2a:88:11:21:98:55:0c:5b:b9:49:97:7d: 
				  9b:f2:7c:01:49:d3:81:ab:dd:d4:f7:f0:68:32:87:b5:ef:52: 
				  b9:4c:d4:b8:ee:f1:ce:aa:e8:ff:f3:68:6c:46:5d:58:7f:bc: 
				  df:5c:1a:95:0f:1a:e5:1c:39:44:ab:cb:f3:f4:75:db:a7:82: 
				  f9:e1:c2:82:be:14:7d:28:4c:9c:50:ca:ee:31:0c:1f:5b:9d: 
				  e3:82:d5:35:35:1a:b5:87:a0:bf:e0:9e:dc:89:51:78:4f:aa: 
				  a6:da:40:5a:a0:7f:8e:e7:51:a5:47:92:47:47:f1:e0:d2:da: 
				  a1:50:76:d7:09:7e:5b:31:77:03:6c:a0:f3:fa:5b:8d:6c:c8: 
				  73:db:39:95 
		 -----BEGIN CERTIFICATE REQUEST----- 
		 MIIC/zCCAecCAQAwWTELMAkGA1UEBhMCVFIxCzAJBgNVBAgMAk5BMREwDwYDVQQH 
		 DAhJc3RhbmJ1bDEMMAoGA1UECgwDTUVSMQswCQYDVQQLDAJQUzEPMA0GA1UEAwwG 
		 bWVyb3BzMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArdTAzTQxtcSF 
		 FKhDzrbdymUtfZdqpzczJn3Rfzmn2RVQnIz92Qeb5xUc/TBB5ri54BNi+Wa7Et5p 
		 nCTWzHpTAXt+xyRcn+Si7d1ZnNNqW3aBR62aHyDTe6eI71mMGnRGue6RfeUwQl9H 
		 USR3V1BvqgfjTnAzfEa1CnZ/HEaBmJHZb5oH6+01bkGNkAG7ztWWuuS21LZrTakN 
		 xOPvKITFURWvOVqLAicNZvyQV8ynbtRU19ylpCsAsbge+43HqN6dx80kbfWla7tH 
		 HEUY/4KtdS9ZpeSCwop70k3/zc8yk8UKb073O/U27Q27iZPhm+TAyEKTE+RaDm/M 
		 GPntP3yFSwIDAQABoGEwXwYJKoZIhvcNAQkOMVIwUDAXBgNVHREEEDAOggZtZXJv 
		 cHOHBMCoUC4wCQYDVR0TBAIwADALBgNVHQ8EBAMCBeAwHQYDVR0lBBYwFAYIKwYB 
		 BQUHAwEGCCsGAQUFBwMCMA0GCSqGSIb3DQEBCwUAA4IBAQAdoUSsxPYruDF+oa4N 
		 /M/JKzo+LstAWjSX6N4k8hg3iYUyGnRIJnmE77Ge3oWjeN0g0siHHiyL9kkBJ3Bu 
		 Qxan0WmvBnLJt7vieX3Ssh16WHSvReuqT2NjARwt1nATMlyu4ro6QmXpKZsbA0VC 
		 MFZikZEqiBEhmFUMW7lJl32b8nwBSdOBq93U9/BoMoe171K5TNS47vHOquj/82hs 
		 Rl1Yf7zfXBqVDxrlHDlEq8vz9HXbp4L54cKCvhR9KEycUMruMQwfW53jgtU1NRq1 
		 h6C/4J7ciVF4T6qm2kBaoH+O51GlR5JHR/Hg0tqhUHbXCX5bMXcDbKDz+luNbMhz 
		 2zmV 
		 -----END CERTIFICATE REQUEST----- 

 
 Although you can use a self-signed certificate, for production environments we recommend that using a public certificate authority. 
 Send the created certificate signing request to the certificate authority. 
 After you receive the server certificate and root certificate, put them in a location where they can be copied to your server(s). 
 Required for SSL server settings and SSL client settings. 


**2. Step**
Change "/C=TR/ST=NA/O=MER/CN=merops" according to above create step
		
		openssl req -new -newkey rsa:4096 -subj "/C=TR/ST=NA/O=MER/CN=merops" -x509 -out /var/opt/hitachi/opscenter/ssl/server/root.pem -nodes -keyout /var/opt/hitachi/opscenter/ssl/server/root.key
		mkdir -p /etc/pki/CA
		mkdir -p /etc/pki/CA/newcerts
		echo "01" > /etc/pki/CA/serial
		echo -n > /etc/pki/CA/index.txt
		vi /etc/pki/CA/san.ext
		
		put in file this text change merops to your vm name and 192.168.80.46 to your vm ip           
		subjectAltName=DNS:merops,IP:192.168.80.46
		
		
		openssl ca -policy policy_anything -cert /var/opt/hitachi/opscenter/ssl/server/root.pem -keyfile /var/opt/hitachi/opscenter/ssl/server/root.key -days 7300 -in /var/opt/hitachi/opscenter/ssl/server/httpsd.csr -out /var/opt/hitachi/opscenter/ssl/server/httpsd.crt -extfile /etc/pki/CA/san.ext
		cd /opt/hitachi/CommonService/utility/bin
		./cssslsetup
		
		 Main menu    Ver:10.9.1-00 
		 1. Create certificate signing request and private key. 
		 2. Set up SSL server. 
		 3. Set up SSL client. 
		 4. Enable/disable certificate verification(optional). 
		 5. Restart services for each product. 
		 Enter a number or q to quit: 2 
		 Specify target products for SSL server setting (separate with commas). 
		 1. All 
		 2. Hitachi Ops Center Common Services 
		 3. Hitachi Ops Center Administrator 
		 4. Hitachi Ops Center Automator 
		 5. Hitachi Ops Center Protector (master) 
		 6. Hitachi Ops Center API Configuration Manager 
		 Enter a number or q to quit [default=1]: 1
		 Determine the storage directory for the private key for Hitachi Ops Center and store the private key to the directory. Please be careful not to delete the private key and the directory. 
		 Enter the absolute path for the stored private key [default=/var/opt/hitachi/opscenter/ssl/server/httpsdkey.pem]: ENTER
		 Determine the storage directory for the server certificate for Hitachi Ops Center and store the server certificate to the directory. Please be careful not to delete the certificate. 
		 Enter the absolute path for the certificate: /var/opt/hitachi/opscenter/ssl/server/httpsd.crt 
		 Is this server certificate issued by an intermediate certificate authority? 
		 1.Yes 
		 2.No 
		 Enter a number [default=2]: 2
		 Determine the storage directory for the root certificate for Hitachi Ops Center and store the root certificate to the directory. Please be careful not to delete the root certificate and the directory. 
		 Enter the absolute path for the stored root certificate: /var/opt/hitachi/opscenter/ssl/server/root.pem 
		 Enter server name [default=merops]: gbopcp201
		 Enter the Hitachi Ops Center Administrator port number: 20961 
		 Enter the credentials for the virtual appliance manager of Hitachi Ops Center Administrator. 
		 User name: sysadmin 
		 Password : 
		 The ECC cryptography key pair set in the Hitachi Ops Center Automator is enabled. 
		 When ECC cryptography is enabled, it is necessary to manually set the ECC cryptography key pair. 
		 Do you want to keep ECC cryptography enabled? 
		 1.Yes(ECC cryptography remain enabled) 
		 2.No(ECC cryptography will be disabled) 
		 Enter a number [default=2]: 2
		 Update settings to configure SSL server? 
		 1.Yes 
		 2.No 
		 Enter a number [default=1]: 1
		 Configuration completed for Hitachi Ops Center Common Services. 
		 Configuration completed for Hitachi Ops Center Administrator. 
		 Configuration completed for Hitachi Ops Center Automator. 
		 Configuration completed for Hitachi Ops Center Protector (master). 
		 Configuration completed for Hitachi Ops Center API Configuration Manager. 
		 KAOP64005-I Setting SSL for server completed successfully. 


**3. Step**

		 Main menu    Ver:10.9.1-00 
		 1. Create certificate signing request and private key. 
		 2. Set up SSL server. 
		 3. Set up SSL client. 
		 4. Enable/disable certificate verification(optional). 
		 5. Restart services for each product. 
		 Enter a number or q to quit: 3 
		 Specify target products for SSL client setting (separate with commas). 
		 1. All 
		 2. Hitachi Ops Center Common Services 
		 3. Hitachi Ops Center Automator 
		 4. Hitachi Ops Center Protector (master) 
		 5. Hitachi Ops Center API Configuration Manager 
		 Enter a number or q to quit [default=1]: 1
		 Determine the storage directory for the root certificate for Hitachi Ops Center and store the root certificate to the directory. Please be careful not to delete the root certificate and the directory. You can skip this input item by pressing the return key in the blank. 
		 Enter the absolute path for the stored root certificate: /var/opt/hitachi/opscenter/ssl/server/root.pem 
		 Truststore ( Product=Hitachi Ops Center Common Services ) : /var/opt/hitachi/CommonService/tls/cacerts 
		 Enter the password for the truststore: changeit
		 Enter the alias name for the truststore: MERCACOM 
		 Truststore ( Product=Hitachi Ops Center Automator ) : /opt/hitachi/Base64/uCPSB11/jdk/lib/security/jssecacerts 
		 Enter the password for the truststore: changeit
		 Retype Enter the password for the truststore: changeit
		 Enter the alias name for the truststore: MERCAAUT 
		 Determine the storage directory for the Active Directory Server certificate for Hitachi Ops Center and store the Active Directory Server certificate to the directory. Please be careful not to delete the Active Directory Server certificate and the directory. You can skip this input item by pressing the return key in the blank. 
		 Enter the absolute path for the stored Active Directory Server certificate: ENTER
		 Do you want to change the certificate used for SSL communication between the Hitachi Ops Center API Configuration Manager server and the storage system? 
		 1.Yes 
		 2.No 
		 Enter a number [default=1]: 2 
		 Select the operation for certificate verification. 
		 1. Enable 
		 2. Disable 
		 Enter a number [default=1]: 1
		 Update settings to configure SSL client? 
		 1.Yes 
		 2.No 
		 Enter a number [default=1]: 1
		 Configuration completed for Hitachi Ops Center Common Services. 
		 Configuration completed for Hitachi Ops Center Automator. 
		 Configuration completed for Hitachi Ops Center Protector (master). 
		 Configuration completed for Hitachi Ops Center API Configuration Manager. 
		 KAOP64008-I Setting SSL for client completed successfully. 

**4. Step**

		 Main menu    Ver:10.9.1-00 
		 1. Create certificate signing request and private key. 
		 2. Set up SSL server. 
		 3. Set up SSL client. 
		 4. Enable/disable certificate verification(optional). 
		 5. Restart services for each product. 
		 Enter a number or q to quit: 5 
		 Specify target products for service restart (separate with commas). 
		 1. All 
		 2. Hitachi Ops Center Common Services 
		 3. Hitachi Ops Center Administrator 
		 4. Hitachi Ops Center Automator 
		 5. Hitachi Ops Center Protector (master) 
		 6. Hitachi Ops Center API Configuration Manager 
		 Enter a number or q to quit [default=1]: 1
		 Restarting services may take some time. 
		 Service restart completed, Product=Hitachi Ops Center Common Services 
		 Service restart completed, Product=Hitachi Ops Center Administrator 
		 Service stop, Product=Hitachi Ops Center Automator 
		 Service start, Product=Hitachi Ops Center Automator 
		 Service restart completed, Product=Hitachi Ops Center Automator 
		 Service start, Product=Hitachi Ops Center Protector (master) 
		 Service restart completed, Product=Hitachi Ops Center Protector (master) 
		 Service stop, Product=Hitachi Ops Center API Configuration Manager 
		 Service start, Product=Hitachi Ops Center API Configuration Manager 
		 Service restart completed, Product=Hitachi Ops Center API Configuration Manager 
		 If you have configured the Hitachi Ops Center Protector (master) or Hitachi Ops Center Administrator SSL client settings, the settings have not been completed yet. 
		 Execute the setupcommonservice command to complete the setting. 
		 Please refer to the target product manual for details. 
		
		 Main menu    Ver:10.9.1-00 
		 1. Create certificate signing request and private key. 
		 2. Set up SSL server. 
		 3. Set up SSL client. 
		 4. Enable/disable certificate verification(optional). 
		 5. Restart services for each product. 
		 Enter a number or q to quit: q 
 
**5. Step**
Register automation to portal
```bash
/opt/hitachi/Base64/bin/hcmds64chgurl -list
/opt/hitachi/Base64/bin/hcmds64chgurl -change https:// gbopcp201:22016 -type Automation
/opt/hitachi/Base64/bin/hcmds64srv -stop -server AutomationWebService
cd /opt/hitachi/Automation/bin
./setupcommonservice -csUri https:// gbopcp201/portal
/opt/hitachi/Base64/bin/hcmds64srv -start
```
