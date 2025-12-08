## 1. SAML config using ubuntu (merubuntu22)
---
---
---



### 1.1. Keycloak Server
---
---

1. Connect keycloak admin console
	 ***https://merubuntu22:8443/***

2. Login with user/password which is created in ***02 - Installation 2.3***
3. Click **Manage Realms**
	 A Realm in Keycloak represents an isolated space where users, applications, roles, and groups are managed. Each Realm has its own configurations and data. This allows for the creation of multiple independent authentication and authorization zones on a single Keycloak server
	 
4. Click **Create Realm**
	 Give a name than click create.(For this server example name opscenter75). This name will be seen at top of OPS Center when you click login with external provider.
	 
5. From **REALMS** list click newly created REALM (opscenter75)
6. You are in opscenter75 realm now.
7. Click **Clients** from left Menu.
8. Click **Create Client** and fill the fields (change merops1104 to your opscenter hostname). Click **Save** when you finished.
```clisp
	Client type                        	: SAML
	Client ID                          	: Any name (hitachi-opscenter in sample) Same name must be used in opscenter keycloak config.
	Name                               	: Just display name (same with client ID in sample)
	Description                        	: Optional
	Always display in UI               	: Off
	Root URL                           	: https://merops1104/portal/
	Home URL                           	: https://merops1104/portal/
	Valid redirect URIs                	: https://merops1104/*
	Valid post logout redirect URIs    	: Leave empty
	IDP-Initiated SSO URL			   	: Leave empty
	IDP-Initiated SSO Relay State      	: Leave empty
	Master SAML Processing URL		   	: https://merops1104/auth/realms/opscenter/broker/merSAML/endpoint (Master SAML will be copied from OPS Center SAML configuration.)
	Node ID format                     	: persistent
	Force node ID format               	: On
	Force POST binding                 	: On
	Force artificial binding           	: Off
	Include AuthnStatement			   	: On
	Include OneTimeUse Condition	   	: Off
	Optimize REDIRECT signing key lookup: Off
	Allow ECP flow						: Off
	Sign documents						: On
	Sign assertions						: On
	Signature algorithm					: RSA_SHA256
	SAML signature key name				: NONE
	Canonicalization method				: EXCLUSIVE
	Rest								: Default
```
9. Click **Client scopes from** right then click **Create client scope**. Fill the fields and click **Save** when you finished.
```clisp
	Name                               : Any Name
	Description                        : Any
	Type                               : None
	Protocol                           : SAML
	Display on consent screen          : On
	Consent screen text                :
	Include in token scope             : On
	Display Order                      :
```
10. Click **mappers** tab in created scope.
11. Configure a new mapper and click **User Attribute**.
12. Create **lastname-mapper** 
```clisp
	Mapper type                        : User Attribute
	Name                               : lastname-mapper
	User Attribute                     : lastName
	Friendly Name                      : 
	SAML Attribute Name                : http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname
	SAML Attribute NameFormat          : BASIC
	Aggregate attribute values		   : Off
```
13. Create **firstname-mapper**
```clisp
	Mapper type                        : User Attribute
	Name                               : firstname-mapper
	User Attribute                     : firstName
	Friendly Name                      : 
	SAML Attribute Name                : http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
	SAML Attribute NameFormat          : BASIC
	Aggregate attribute values		   : Off
```
14. Configure a new mapper and click **User Property**.for username 
```clisp
	Mapper type                        : User Property
	Name                               : username
	Property                           : username
	Friendly Name                      : username
	SAML Attribute Name                : uid
	SAML Attribute NameFormat          : BASIC
	Aggregate attribute values		   : Off
``` 
14. Configure a new mapper and click **User Property**.for email
```clisp
	Mapper type                        : User Property
	Name                               : email
	Property                           : email
	Friendly Name                      : 
	SAML Attribute Name                : http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
	SAML Attribute NameFormat          : BASIC
	Aggregate attribute values		   : Off
```
15. Click **Clients** from left and choose client created in step 8.
16. Go to **Client Scopes** tab in **Client Details** and **Add client scope** which is created in step 9 as default.
17. Choose user from left and add user.
18. Fill following fields and click **Create**
```clisp
	Username
	Email
	First Name
	Last Name
```
19. Go to **Credentials** tab and **Set password** for user.
	 Turn off Temporary.

20. Go to **Attributes** tab and add the following attribute. If you do not add this attribute Keycloak puts and **generated ID** here and OPS Center shows id instead of username.
```clisp
	Key 	: saml.persistent.name.id.for.hitachi-opscenter
	Value	: name of user
```

If you want to enable OTP (One Time Password) option like Google Authenticator,Okta or freeOTP go on with section 1.1.1 and for LDAP follow section 1.1.2 on "**03 - Keycloak Configuration - OPENID**", otherwise keycloak config finished please go on OPS Center configuration.




### 1.2. OPS Center Keycloak client configuration
---
---

1. Login **OPS Center** with **ssh**
2. Run enable Keycloak command. It will ask a password for **idpadmin** user.
```bash
	/opt/hitachi/CommonService/utility/bin/csembeddedkeycloak -enable
```
3. Copy Keycloak.local.crt certificate file (generated in section ***02 - Installation 2.2**) to OPS Center **/var/opt/hitachi/CommonService/tls/** folder.
4. Import certificate to Common Service trust store. This is all in one line command. It will also ask trust store password. Default Password is **“changit”**
```bash
	keytool -importcert -alias keycloak22 -keystore /var/opt/hitachi/CommonService/tls/cacerts -file /var/opt/hitachi/CommonService/tls/keycloak.local.crt
```
   To list imported certificates
```bash
	keytool -list -keystore /var/opt/hitachi/CommonService/tls/cacerts
```
   Restart common service
```bash
	systemctl restart csportal
```
5. Login **OPS Center Portal** with sysadmin or **admin** privilege user.
6. Go to **Manage users**. Click **Identity providers (Other)** on the left. Click **Embedded Keycloak**
7. Login to embedded Keycloak with **idpadmin** user and password
8. You will be **Identity providers** section by default. Click on **SAML v2.0**
9. Fill the fields with your Keycloak server details on **Setting** tab. Change merubuntu22 with your Keycloak server hostname and opscenter75 with your REALM name created in section 1.1 step 8. Click **Save** when you finished.
```clisp
	Redirect URI                        	: OPS Center will fill automatically
	Alias                               	: Any name (merSAML in sample)
	Display name                        	: Any name (MER-SAML in sample)
	Display order                       	: Leave blank
	Service provider entity ID				: Client name created in Keycloak server (hitachi-opscenter in sample)
	Identity provider entity ID				: https://merubuntu22:8443/realms/opscenter75
	Single Sign-On service URL				: https://merubuntu22:8443/realms/opscenter75/protocol/saml
	Artifact Resolution service URL			: Leave empty
	Single logout service URL				: https://merubuntu22:8443/realms/opscenter75/protocol/saml
	Backchannel logout						: Off
	Send 'id_token_hint'in logout requests	: On
	Send 'client_id'in logout requests		: Off
	NameID policy format					: Persistent
	Principal type							: Subject NameID
	Allow create							: On
	HTTP-POST binding response				: Off
	ARTIFACT binding response				: Off
	HTTP-POST binding for AuthnRequest		: Off
	HTTP-POST binding logout 				: Off
	Want AuthnRequest signed				: Off
	Want Assertions Signed					: Off
	Force authentication					: Off
	Validate Signatures						: Off
	Sign service provider metadata			: Off
	Pass subject							: Off
	Allowed clock skew						: 0
	Attribute Consuming Service Index		: 0
	Attribute Consuming Service Name 		:
	Comparison								: exact
	AuthnContext ClassRefs					: 
	AuthnContext DeclRefs					: 
	Share tokens							: Off
	Share tokens readable					: Off
	Trust Email								: Off
	Account linking only					: Off
	Store tokens							: Off
	Store tokens readable					: Off
	Trust Email								: Off
	Account linking only					: Off
	Hide on login page						: Off
	First login flow override				: First login flow override
	Post login flow							: None
	Sync mode								: Legacy
	Case-sensitive username					: Off
```
10. Click **Mappers** tab and click **Add mapper** button. Id field will not exist while adding mappers.it will be assigned automatically.
	   Users from Keycloak will be assigned to opscenter-users group default. If you would like to assign any user admin role, you must login OPS Center with sysadmin and assign admin role to user from Manage user/users screen.
	
	1. Email-mapper
```clisp
	Name                               : email-mapper
	Sync mode override                 : Force
	Mapper type						   : Attribute importer
	Attribute Name					   : http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
	Friendly Name                      : 
	Name Format						   : ATTRIBUTE_FORMAT_BASIC
	User Attribute Name				   : email
```

   2. Lastname-mapper
```clisp
	Name                               : lastname-mapper
	Sync mode override                 : Force
	Mapper type						   : Attribute importer
	Attribute Name					   : http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname
	Friendly Name                      : 
	Name Format						   : ATTRIBUTE_FORMAT_BASIC
	User Attribute Name				   : lastName
```

   3. Firstname-mapper
```clisp
	Name                               : firstname-mapper
	Sync mode override                 : Force
	Mapper type						   : Attribute importer
	Attribute Name					   : http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
	Friendly Name                      : 
	Name Format						   : ATTRIBUTE_FORMAT_BASIC
	User Attribute Name				   : firstName
```

   4. Hardcoded-group
```clisp
	Name                               : hardcoded-group
	Sync mode override                 : Force
	Mapper type						   : Hardcoded Group
	Group							   : /opscenter-users
```	


All setup completed.