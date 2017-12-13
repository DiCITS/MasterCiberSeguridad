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

![Schema](https://github.com/DiCITS/MasterCiberSeguridad/blob/master/extras/images/schema2.png?raw=true)

Each user has 10 ports within the range ```14XXX```. The rank assigned to each user will be provided during the practice class, always being a contiguous range of ports.

To access the services, depending on the service we will use the corresponding application. For example, if it is a web service that has been installed/published in port 14067, to see the service, we can see it in the browser: ```http://bahia.ugr.es:14067/``` and this will redirect traffic to the service that is running in that port within ```bahia.ugr.es```.

Table with port assignment:

| 	User      |  bahia.ugr.es   |   docker.ugr.es | 
|-------------|-----------------|-----------------|
| cs_75XXXXX2 |  14000 al 14009	| 	30208,30209   | 
| cs_76XXXXX5 |  14010 al 14010	| 	30210,30211   | 
| cs_76XXXXX2 |  14020 al 14029	| 	30212,30213   | 
| cs_44XXXXX9 |  14030 al 14039	| 	30214,30215   | 
| cs_31XXXXX3 |  14040 al 14049	| 	30216,30217   | 
| cs_14XXXXX8 |  14050 al 14059	| 	30218,30219   | 
| cs_76XXXXX7 |  14060 al 14069	| 	30220,30221   | 
| cs_75XXXXX4 |  14070 al 14079	| 	30222,30223   | 
| cs_75XXXXX6 |  14080 al 14089	| 	30224,30225   | 
| cs_15XXXXX7 |  14090 al 14099	| 	30226,30227   | 
| cs_23XXXXX6 |  14100 al 14109	| 	30228,30229   | 
| cs_14XXXXX5 |  14110 al 14119	| 	30230,30231   | 
| cs_76XXXXX4 |  14120 al 14129	| 	30232,30233   | 
| cs_46XXXXX1 |  14130 al 14139	| 	30234,30235   | 
| cs_20XXXXX7 |  14140 al 14149	| 	30236,30237   | 
| cs_77XXXXX4 |  14150 al 14159	| 	30238,30239   | 

Ports in ```docker.ugr.es``  are redirected to ```389``` and ```636``` (i.e: first ```30208``` -> ```389``` ; second ```30209``` -> ```636```).


### Next steps

Once understood the scheme of work and access to the servers, the next thing to do is work with Docker containers on ```bahia.ugr.es```

[Starting docker](../Docker/starting_docker.md)



