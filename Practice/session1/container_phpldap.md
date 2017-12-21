
Create in your home folder a folder i.e. ```web```:

```mkdir web```

Create a new container:

```docker run -p 14XXX:80 -v /home/cs_XXXXXX/web/:/var/www/html/ --name <your_container_name> -d vaniltonpinheiro/apache-php-ldap```

Go to and check if container is working:

```https://docker.ugr.es:14006/```



Go to the folder:

```cd /home/cs_XXXXXX/web/```

And download authenticacion aplication in PHP:

```wget https://raw.githubusercontent.com/DiCITS/MasterCiberSeguridad/master/extras/authentication.php```

Edit ```autentication.php``` file downloaded and change all occurences:
- server ```docker.ugr.es```
- port ```389``` and
- ```dc=openstack,dc=org ```


Change your user password in LDAPDocker  (if you dont remember):

```ldappasswd -s <newpassword> -W -D "cn=admin,dc=openstack,dc=org" -x "cn=<your_user_name>,ou=Users,dc=openstack,dc=org"```

Go to 

```https://docker.ugr.es:14006/autentication.php```

Try your credentials, and check if user is authenticated.




