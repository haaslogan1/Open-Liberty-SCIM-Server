# Open-Liberty-SCIM-Server
Configuration of an 23.0.0.12 Open Liberty server deploying the System for Cross-Domain Identity Management (SCIM) service.

# Open Liberty Installation
On most Linux distributions with wget install, simply run:
wget https://public.dhe.ibm.com/ibmdl/export/pub/software/openliberty/runtime/release/**23.0.0.12/openliberty-23.0.0.12.zip**
unzip openliberty-23.0.0.12

Of course, you can tweak the URL to get a different release.

# SCIM Installation
From the bin directory, run the following (replace 23.0.0.12 with your current Open Liberty version):
featureUtility installFeature scim-1.0 --featuresbom=com.ibm.websphere.appserver.features:features:**23.0.0.12**

# LDAP Configuration
For an extremely simple LDAP configuration using all the defaults, simply add the corresponding environment variables in server.env from this line:
<ldapRegistry id="LDAP1" host=${host} port="636" ldapType=${ldapType} baseDN=${baseDN} bindDN=${bindDN} bindPassword=${bindPassword} sslEnabled="true">

Since SSL is enabled, the LDAP server's root and intermediate certificates need to be added to the trust store. 

The server.env file should be encrypted as it will store the bind user's password.

# Remove the Basic Registry

Before going to production, ensure the Basic Registry is removed from the configuration. The basic registry is only included in this configuration to make SCIM easily accessibly via the 'admin' administrator (without having to know an LDAP user's credentials).
