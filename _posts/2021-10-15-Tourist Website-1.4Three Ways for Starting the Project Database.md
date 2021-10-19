---
title: Three Ways for Starting the Project Database
tags: .Netcore/API/MVC
author: Anning Mao
---

## 1. Visual Studio's own database

If you are using a Windows operating system, then your visual studio comes with SQL Express service. 

View->SQL Server Object Explorer

![1.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/4.Deploy%20mssql%20On%20Docker/1.1.png)

![1.2](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/4.Deploy%20mssql%20On%20Docker/1.2.png)

This way is simple and easy to use, you can use it without any configuration

## 2. SQL server express

It is not recommended, because it is very troublesome. Installing an independent SQL server express takes a lot of resources and requires a separate installation of SQL server management Studio (ssms). The most important thing is that it is no different from the one that comes with visual studio. We have cross-platform requirements, but the windows database mssql cannot run on the mac. So I recommend the third method.

## 3. Deploy mssql on docker

With docker, we can deploy mssql cross-platform and containerized

### Install Docker on windows:

First you have to check your windows version. Because the virtualization service needs to be turned on, the version should preferably be a professional version or a professional workstation version.

Open Hyper-v in windows function

![3.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/4.Deploy%20mssql%20On%20Docker/3.1.png)



Download Docker here https://www.docker.com/get-started

![3.2](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/4.Deploy%20mssql%20On%20Docker/3.2.png)



check it

![3.3](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/4.Deploy%20mssql%20On%20Docker/3.3.png)



### Run your container

*Pull mssql image and check it*

``` bash
docker pull mcr.microsoft.com/mssql/server
```

``` bash
docker images
```

![3.4](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/4.Deploy%20mssql%20On%20Docker/3.4.png)



*Run this image*

``` bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=Yourpassword" -p 1433:1433 -d mcr.microsoft.com/mssql/server
```

Two environment variables are configured when running:

​	1. Accept the end user license agreement

​		-e "ACCEPT_EULA=Y" for windows

​		-e ’ACCEPT_EULA=Y‘ for mac

​	2.Account(SA) and password(Yourpassword) 

and the default port is 1433



*Possible problems*

If you encounter the following problems, the reason is most likely that port 1433 is occupied by hyper-v

`provider: Named Pipes Provider, error: 40 - Unable to open connection to SQL Server) (Microsoft SQL Server，error: 2)`

Solution:

``` bash
dism.exe /Online /Disable-Feature:Microsoft-Hyper-V
netsh int ipv4 add excludedportrange protocol=tcp startport=1433 numberofports=1
dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All
```



### Connect to your mssql server in vs

SQL Server Object Explorer->add SQL Server

![3.5](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/4.Deploy%20mssql%20On%20Docker/3.5.png)

Then you could connect to the mssql server on Docker.

