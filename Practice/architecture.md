**Course 2017-2018**

[Master CyberSecurity -- University of Granada](http://ucys.ugr.es/master-propio-en-ciberseguridad/)

![LogoHeadMasterCES](https://sites.google.com/site/manuparra/home/logo_master_ciber.png)


[UGR](http://www.ugr.es) | [DICITS](http://dicits.ugr.es) | [SCI2S](http://sci2s.ugr.es) | [DECSAI](http://decsai.ugr.es)

Manuel J. Parra Royón (manuelparra@decsai.ugr.es) & José. M. Benítez Sánchez (j.m.benitez@decsai.ugr.es)


This is the repository for:

## Module: Network and Systems Protection and the course Security in Operating Systems

In this section you can consult the cluster structure to be used in the subject.

### Access to the servers

Two servers will be accessed:

- ```bahia.ugr.es```
- ```docker.ugr.es```

To access the clusters it is necessary to use SSH, if you use Windows, you will need the SSH Putty application or if you use Linux, the ssh command is already integrated.

The connection data to the clusters are:

- Server: ```bahia.ugr.es```   or    ```docker.ugr.es```
- Port: ```22```
- Username: ```<your assigned username>```
- Password: ```<your assigned password>```


To connect by ssh to the servers is used:

```ssh <your assigned username>@bahia.ugr.es```

or 

```ssh <your assigned username>@docker.ugr.es```

Use your assigned password when asked.


### Port diagram enabled for each user:

Each user has a range of 10 TCP ports and 10 open UDP ports on X and Y. These ports will be used to make available services that are enabled from containers or virtual machines.

So ports that are used locally on bahia.ugr.es and ```docker.ugr.es``` for services must always go in the range that has been set for each user.

This is the schema of the port range:

![Schema](https://github.com/DiCITS/MasterCiberSeguridad/blob/master/extras/images/schema.png?raw=true)

Each user has 10 ports within the range ```14XXX```. The rank assigned to each user will be provided during the practice class, always being a contiguous range of ports.

To access the services, depending on the service we will use the corresponding application. For example, if it is a web service that has been installed in port 14067, to see the service, we can see it in the browser: ```http://bahia.ugr.es:14067/``` and this will redirect traffic to the service that is running in that port within ```bahia.ugr.es```.



