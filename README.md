# Installing Kaltura Docker container 
This guide describes docker container installation of an all-in-one Kaltura server and applies to all docker engines on Linux, windows and mac.


#### Table of Contents

[Prerequisites](README.md#Prerequisites)

[Step-by-step Installation](README.md#step-by-step-installation)

[Unattended Installation](README.md#unattended-installation)

## Prerequisites

Installed [Docker Engine](https://docs.docker.com/engine/installation).

## Step-by-step Installation

### Pre-Install notes
* This install guide assumes that you ran kaltura/server container.
* When installing, you will be prompted for each server's resolvable hostname. Note that it is crucial that all host names will be resolvable by other servers in the cluster (and outside the cluster for front machines). Before installing, verify that your `/etc/hosts` file is properly configured and that all Kaltura server hostnames are resolvable in your network.

### Running the image
Kaltura requires certain ports to be open for proper operation. [See the list of required open ports](kaltura-required-ports.md).
The container already documented to expose 80, 443 and 1935 ports, make sue to run it using -p option to enable access to these ports.

### Start Kaltura container
```bash
docker run -d --name=kaltura -p 80:80 -p 443:443 -p 1935:1935 -p 88:88 -p 8443:8443 kaltura/server
```

### Start of Kaltura Configuration

```bash
# docker exec -i -t kaltura /root/install/install.sh MYSQL_ROOT_PASSWD_HERE
``` 

Your install will now automatically perform all install tasks. You just need to paas some details of the questions asked during installation process.

The below is a sample question answer format, replace the input marked by <> with your own details:

```sh
[Email\NO]: "<your email address>"
CDN hostname [kalrpm.lcl]: "<your hostname>"
Apache virtual hostname [kalrpm.lcl]: "<your hostname>"
Which port will this Vhost listen on [80]?: "<443>"
DB hostname [127.0.0.1]: "<127.0.0.1>"
DB port [3306]: "<3306>"
MySQL super user [this is only for setting the kaltura user passwd and WILL NOT be used with the application]: "<root>"
MySQL super user passwd [this is only for setting the kaltura user passwd and WILL NOT be used with the application]: "<your root password>"
Analytics DB hostname [127.0.0.1]: "<127.0.0.1>"
Analytics DB port [3306]: "<3306>"
Sphinx hostname [127.0.0.1]: "<127.0.0.1>"

Secondary Sphinx hostname: [leave empty if none] "<empty>"

VOD packager hostname [kalrpm.lcl]: "<kaltura-nginx-hostname>"

VOD packager port to listen on [88]: 

Service URL [http://kalrpm.lcl:443]: "<http://your-hostname:443>"

Kaltura Admin user (email address): "<your email address>"
Admin user login password (must be minimum 8 chars and include at least one of each: upper-case, lower-case, number and a special character): "<your kaltura admin password>"
Confirm passwd: "<your kaltura admin password>"

Your time zone [see http://php.net/date.timezone], or press enter for [Europe/Amsterdam]: "<your timezone>"
How would you like to name your system (this name will show as the From field in emails sent by the system) [Kaltura Video Platform]? "<your preferred system name>"
Your website Contact Us URL [http://corp.kaltura.com/company/contact-us]: "<your contact URL>"
'Contact us' phone number [+1 800 871 5224]? "<your phone number>"

Is your Apache working with SSL?[Y/n]: "<Y>"
Please input path to your SSL certificate: "</path/to/my/ssl/certificate>"
Please input path to your SSL key: "</path/to/my/ssl/key>"
Please input path to your SSL chain file or leave empty in case you have none: "</path/to/my/ssl/chainfile>"
Which port will this Vhost listen on? [443] "<443>"
Please select one of the following options [0]: "<0>"
```


**Your Kaltura installation is now complete.**

## Unattended Installation
All Kaltura scripts accept an answer file as their first argument.
In order to preform an unattended [silent] install:

 - Create **config.ans** file according to [template](kaltura.config.template.ans).
 - Copy to file into the container: 
```bash
# docker cp config.ans kaltura:/root/install/
``` 
 - Run the installer, it will automatically search for answers file in this path.
```
# docker exec -i -t kaltura /root/install/install.sh
```

**Note:** answer files contain a mysql root password param called SUPER_USER_PASSWD. Therefore, do NOT pass a MySQL root passwd as an argument when performing an unattended installation.
