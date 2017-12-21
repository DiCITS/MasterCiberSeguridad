# Installing SLDAP service

Go to docker.ugr.es.

```ssh cs_XXXXXX@docker.ugr.es```

Login with your opennebula user:

```oneuser login .....```

Connect to you Virtual Machine:

```ssh root@192.168....```

Remember your VM IP. It can be reached using the comands:

```
onevm list
onevm show <ID>
```

And review your assigned IP.

Once inside the Virtual Machine, we will install the LDAP server:

Install OpenLDAP application and services. More info and details: https://www.centos.org/docs/5/html/Deployment_Guide-en-US/s1-ldap-quickstart.html

```yum -y install *openldap* migrationtools```

It will install openldap packages and migrationtool (migrate local users to LDAP).

Create a LDAP root passwd for administration purpose.

```slappasswd```

This command will ask you about your LDAP admin password. It will be used for each elevated operation (admin operations).

Copy the hashed password returned by last command.

Edit the OpenLDAP Server Configuration

```cd /etc/openldap/slapd.d/cn=config```

```vi olcDatabase={2}hdb.ldif```

Change the variables of "olcSuffix" and "olcRootDN" according to our domain as below.

```
olcSuffix: dc=ugr,dc=es
olcRootDN: cn=Manager,dc=ugr,dc=es
```

Add the below three lines additionally in the same configuration file.

```
olcRootPW: <PASSWORD STRING GENERATED with slappasswd>
olcTLSCertificateFile: /etc/pki/tls/certs/ugr.pem
olcTLSCertificateKeyFile: /etc/pki/tls/certs/ugrkey.pem
```

NOTE: <PASSWORD STRING GENERATED with slappasswd> must be the hashed password.

Change monitor privileges

```vi /etc/openldap/slapd.d/cn=config/olcDatabase={1}monitor.ldif```

Go to line starting with olcAccess and change values with ```cn=Manager,dc=ugr,dc=es```:

```olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external, cn=auth" read by dn.base="cn=Manager,dc=ugr,dc=es" read by * none```

Check configuration

```slaptest -u```

NOTE: Don't mind warnings

Enable services

```
systemctl start slapd
systemctl enable slapd
```

Configure Database

```
cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
chown -R ldap:ldap /var/lib/ldap/
```

Add default schemas

Those schemas are required:

```
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
```
Create certificates for LDAP

```
openssl req -new -x509 -nodes -out /etc/pki/tls/certs/learnitguideldap.pem -keyout /etc/pki/tls/certs/learnitguideldapkey.pem -days 365
```
Provide your details to generate the certificate.

Common name will be: ugr.es


Base

```touch /root/base.ldif```

and add:

```
dn: dc=ugr,dc=es
objectClass: top
objectClass: dcObject
objectclass: organization
o: ugr es
dc: ugr

dn: cn=Manager,dc=ugr,dc=es
objectClass: organizationalRole
cn: Manager
description: Directory Manager

dn: ou=People,dc=ugr,dc=es
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=ugr,dc=es
objectClass: organizationalUnit
ou: Group
```


and then execute:

```ldapadd -x -W -D "cn=Manager,dc=ugr,dc=es" -f /root/base.ldif```

This command will add OU=People, OU=Group, etc.


Creating users:

Create a file:
```users.ldif``` -> fill data from out last tutorial: https://github.com/manuparra/docker_ldap#training-with-ldap

```ldapadd -x -W -D "cn=Manager,dc=ugr,dc=es" -f /root/users.ldif``` 

For instance (change yhour values):

``` 
dn: uid=myuser,ou=People,dc=ugr,dc=es
uid: myuser
cn: myuser
sn: myuser
mail: myuser@ugr.es
objectClass: person
objectClass: organizationalPerson
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: top
objectClass: shadowAccount
userPassword: {crypt}$6$aRkYTYsY$7FcXPmehiisAhbTiF3kDVo7g.EOKfpvhkrYExK5Y7wNq0rh0JJehbKDZKbUVxoF2hO0KfP1bmWPhXvq9BIxJT/
shadowLastChange: 17149
shadowMin: 0
shadowMax: 99999
shadowWarning: 7
loginShell: /bin/bash
uidNumber: 1000
gidNumber: 1000
homeDirectory: /home/myuser
``` 

*NOTE: Remember change user password with LDAP