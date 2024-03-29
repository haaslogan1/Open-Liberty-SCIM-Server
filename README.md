# Open-Liberty-SCIM-Server
Configuration of an 23.0.0.12 Open Liberty server deploying the System for Cross-Domain Identity Management (SCIM) service.

# Open Liberty Installation
On most Linux distributions with wget install, simply run:
```
wget https://public.dhe.ibm.com/ibmdl/export/pub/software/openliberty/runtime/release/**23.0.0.12/openliberty-23.0.0.12.zip**
unzip openliberty-23.0.0.12
```

Of course, you can tweak the URL to get a different release.

# SCIM Installation
From the bin directory, run the following (replace 23.0.0.12 with your current Open Liberty version):
```
featureUtility installFeature scim-1.0 --featuresbom=com.ibm.websphere.appserver.features:features:**23.0.0.12**
```

# LDAP Configuration
For an extremely simple LDAP configuration using all the defaults, simply add the corresponding environment variables in server.env from this line:
```
<ldapRegistry id="LDAP1" host=${host} port="636" ldapType=${ldapType} baseDN=${baseDN} bindDN=${bindDN} bindPassword=${bindPassword} sslEnabled="true">```
```
Since SSL is enabled, the LDAP server's root and intermediate certificates need to be added to the trust store. 

The server.env file should be encrypted as it will store the bind user's password.

# Example invoking the SCIM API to return User Attributes
```
curl -vks GET "https://localhost:9443/ibm/api/scim/Users/uid=X" -u admin:admin
```

Host is set to localhost. So, this command must be run on the machine where Liberty is running. By default, Liberty includes a firewall not accepting any inbound traffic outside localhost. To change this, set the [host property](https://openliberty.io/docs/latest/reference/config/virtualHost.html)

Port is set to 9443. This is the HTTPS port specified in the server.xml.

The end of the URL (_uid=X_) should be replaced with the user that you would like to query as a name-value pair: _attribute=usersValue_.

The -u option specificies the user credentials (in the HTTP GET call) required to access the protected API endpoint. In this case, we are using a basic registry admin user defined in the server.xml.

# Remove the Basic Registry

Before going to production, ensure the Basic Registry is removed from the configuration. The basic registry is only included in this configuration to make SCIM easily accessibly via the 'admin' administrator (without having to know an LDAP user's credentials).
