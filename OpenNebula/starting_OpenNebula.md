Máster en CiberSeguridad 2016-2017. Universidad de Granada.

Manuel J. Parra Royón (manuelparra@decsai.ugr.es) & José. M. Benítez Sánchez (j.m.benitez@decsai.ugr.es)

Soft Computing and Intelligent Information Systems

Distributed Computational Intelligence and Time Series Lab

University of Granada

Table of Contents
=================

   * [Starting with OpenNebula](#starting-with-opennebula)
      * [First steps with OpenNebula](#first-steps-with-opennebula)
         * [onemarket](#onemarket)
         * [oneimage](#oneimage)
         * [onevnet](#onevnet)
         * [onetemplate](#onetemplate)
         * [onevm](#onevm)
      * [onehost](#onehost)
      * [Deploy Virtual Machines with OpenNebula](#deploy-virtual-machines-with-opennebula)
         * [Authentication in OpenNebula](#authentication-in-opennebula)
         * [List available images on our OpenNebula system](#list-available-images-on-our-opennebula-system)
         * [Listing Virtual Networks for your user](#listing-virtual-networks-for-your-user)
         * [Create a Virtual Machine instance template](#create-a-virtual-machine-instance-template)
         * [Launch Virtual Machine instance](#launch-virtual-machine-instance)
         * [How to connect  (SSH) to the created Virtual Machine](#how-to-connect--ssh-to-the-created-virtual-machine)
      * [Manage Virtual Machine on SunStone Website](#manage-virtual-machine-on-sunstone-website)


# Starting with OpenNebula

## First steps with OpenNebula

OpenNebula commands:

```
onemarket         onevm             oneacct           onedatastore
onegroup          onetemplate       onevnet           oneacl
onedb             onehost           oneuser           onezone
onecluster        oneimage          onevcenter        
```

### onemarket

It allows to list the images of Operating systems, data, etc. that we can import to add to our cloud available from a MarketPlace

List of OSs available to run:

```
onemarket list
```

### oneimage

It allows to list the images that are in the system ready to be used for a display of virtual machines

List of imported images available to work:


```
oneimage list
```

### onevnet

Allows you to manage the virtual networks that have been created.

List of virtual networks created:

```
onevnet show <ID>
```

### onetemplate

It allows to manage templates of generation of virtual machines, so that we can create templates that have certain attributes of memory, capacity, etc. etc.

List of available templates

```
onetemplate list
onetemplate show <ID>
```

It is normal to appear empty since we have not yet made any template. We will create it later.

### onevm

It allows to manage the virtual machines launched, to know the state, to modify the state, to know the IP / Network, resources, etc., etc.

List of virtual machines available:

```
onevm list
```

It is normal to appear empty since we have not yet launched any virtual machine. We will create it later.

### onehost

Allows you to manage the hosts and resources of the cloud created with OpenNebula

List of hosts of the system and its state:

```
onehost list
```

It appears empty since HOSTS are managed from a privileged user profile.


## Deploy Virtual Machines with OpenNebula

Access to the docker ugr server:

```
ssh manuparra@docker....
```

### Authentication in OpenNebula

This is the first step before working with OpenNebula:

```
oneuser login <youruserlogin> --ssh --force
```

for example:

```
oneuser login manuparra --ssh --force
```

If this command works fine, you are allow to use and connect with OpenNebula.

### List available images on our OpenNebula system

```
oneimage list
```

It will show:


```
 ID USER       GROUP      NAME            DATASTORE     SIZE TYPE PER STAT RVMS
   8 oneadmin   users      CentOS-6.5-one- default        10G OS    No used    1
   9 oneadmin   users      CentOS-7        default        10G OS    No rdy     0
  10 oneadmin   users      Ubuntu-14.04    default        10G OS    No rdy     0
  11 oneadmin   users      Hadoop 1.2 Mast default       1.3G OS    No rdy     0
  12 oneadmin   users      Hadoop 1.2 Slav default       1.3G OS    No rdy     0
  ...
```

Images are referenced by *"ID"* or *"NAME"*. You can use those IDENTIFIERS to reference list elements.

If we want to use CENTOS7, we need the ID => 9 o NAME => "CentOS-7".

### Listing Virtual Networks for your user

Now we have to verify that we have created our Virtual Network to be able to launch the machines within a correct IP address space:

```
onevnet list
```

and look forward for your Virtual Network

```
 ID USER            GROUP        NAME                CLUSTER    BRIDGE   LEASES
   0 oneadmin        oneadmin     private             -          br0           0
   1 patriciajimeno  users        private1            -          br0           0
   2 alejandroalonso users        alejandroalonso_vne -          br0           0
   3 arantzazulopez  users        arantzazulopez_vnet -          br0           0
   ...
```

Remember your Virtual Network **ID** or Virtual Network **NAME**.

### Create a Virtual Machine instance template

We use ``onetemplate`` with this syntax:

```
onetemplate create --name <nametemplate> --cpu <numcpu> --vcpu <numvcpu> --memory <mem> --arch x86_64 --disk <imageid> --nic <idvnet> --vnc --ssh --net_context
```

for example:

```
onetemplate create --name "Plantilla_CentOS" --cpu 1 --vcpu 1 --memory 512 --arch x86_64 --disk 9 --nic "manuelparra_vnet" --vnc --ssh --net_context
```

Those are the parameters explained:

- --name :   Indica el nombre plantilla a crear. Debe ser un texto identificativo de la MV a crear.
- --cpu : El número de CPUS que vamos a utilizar. Debe ser un número entero.
 - --vcpu : El número de CPUs Virtuales que se utilizarán. Debe ser un número entero.
- --memory : La memoria RAM en MB que se usará para la Maquina Virtual.
- --arch:  La arquitectura que se va a usar. Debe estar en concordancia con la imagen que se 			utilizará; puede ser x86_64, x86, … 
- --disk: El ID del disco o imagen que vamos a usar. El identificador de la lista se puede ver ver en  		oneimage list y puede ser un ID o bien el NOMBRE del disco.
- --nic : El nombre de la RedVirtual que usaremos. El identificador se puede consultar utilizando el 		mandato onevnet list, desde donde se consulta el ID o NOMBRE de la RED a 			usar.
- --ssh: acceso via ssh.
- --net_context:   Usaremos el contextos para la imagen.

To verify if the template was created:


```
onetemplate list
```



If we want to delete the template, first get the selected ID or NAME and then:

```
onetemplate delete <ID>
```

### Launch Virtual Machine instance

First, list your Virtual Machines list:

```
onevm list
```

and check again the list of my templates:

```
onetemplate list
```

And now, we create a new instance of a Virtual Machine:

```
onetemplate instantiate <IDtemplate>
```

for example:


```
onetemplate instantiate 63
```

It will returns an ID of the created Virtual Machine.


Because the start of the virtual machine takes time, we will see the state at each moment, so check list of your Virtual Machines:

```
onevm list
```

It will show:

```
ID USER     GROUP    NAME            STAT UCPU    UMEM HOST             TIME
5 manuelpa users    Plantilla_CentO runn    0    512M noded07      0d 01h13
```

In the STAT column we have the current state. RUNNING = RUN
And information about Memory, HOST, etc.

### How to connect  (SSH) to the created Virtual Machine


Check the ID of your Virtual Machine:

```
onevm list
```

and get your Virtual Machine ID:

```
onevm show <ID> 
```

```
onevm show 60
```

It will returns all information about your Virtual Machine.

The part that interests us of the details is to know that IP has been assigned to this machine created:

```
CONTEXT=[
  DISK_ID="1",
  ETH0_DNS="150.214.191.10",
  ETH0_GATEWAY="192.168.12.1",
  ETH0_IP="192.168.12.148",
  ETH0_MAC="02:00:c0:a8:0c:94",
  NETWORK=“YES",
  ....
```

So, our Virtual Machine IP to connect with SSH is: ``192.168.12.148``

We try to connect to the Virtual Machine:

```
ssh root@192.168.12.148
```

Verify if you are connected to Internet:

```
ping -c2 google.com
```

## Manage Virtual Machine on SunStone Website

Go to: http://docker.ugr.es:9869/ and follow the next steps:

- Connect to docker server ugr: ``ssh manuparra@.....``
- Execute: ``cat .one/one_auth`` it will show your SunStone credentials in this format: ``<user>:<password>`` , for example: ``manuparra:7374j31g74hd7234``
- Copy the corresponding part of password and paste http://docker.ugr.es:9869/ login and password.




