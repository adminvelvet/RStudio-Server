### Install and monitor RStudio server

RStudio Server enables you to provide a browser based interface (the RStudio IDE) to a version of
R running on a remote Linux server. Deploying R and RStudio on a server has a number of benefits,
including:  
- The ability to access R sessions from any computer in any location;
- Easy sharing of code, data, and other files with colleagues;
- Allowing multiple users to share access to the more powerful compute resources (memory,
processors, etc.) available on a well equipped server; and
- Centralized installation and configuration of R, R packages, TeX, and other supporting
libraries.

This manual describes RStudio Server Professional Edition, which adds many enhancements to the
open-source version of RStudio Server, including:  
- The ability to run multiple concurrent R sessions per-user.
- Flexible use of multiple versions of R on the same server.
- Project sharing for easy collaboration within workgroups.
- Load balancing for increased capacity and higher availability.
- An administrative dashboard that provides insight into active sessions, server health, and
monitoring of system-wide and per-user performance and resource metrics;
- Authentication using system accounts, ActiveDirectory, LDAP, or Google Accounts;
- Full support for PAM (including PAM sessions for dynamically provisioning user resources);
- Ability to establish per-user or per-group CPU priorities and memory limits;
- HTTP enhancements including support for SSL and keep-alive for improved performance;
- Ability to restrict access to the server by IP;
- Customizable server health checks; and
- Suspend, terminate, or assume control of user sessions; Impersonate users for assistance and
troubleshooting.

I - Cent OS

> 0- Install R

```sh
sudo yum install R
```

> 1- Download and install R Studio

```sh
# Download
wget https://download2.rstudio.org/rstudio-server-rhel-0.99.903-x86_64.rpm

# Install
sudo yum install --nogpgcheck rstudio-server-rhel-0.99.903-x86_64.rpm
```

> 2- Configuring the Server
RStudio is configured by adding entries to two configuration files:
- /etc/rstudio/rserver.conf
- /etc/rstudio/rsession.conf
```sh
# Create server configuration file
nano /etc/rstudio/rserver.conf
```
> Add the following code
2-1- Server configuration
```sh
# RStudio port
www-port=8787
# Allow trafic from a specific IP
www-address=0.0.0.0 # world wide
# Add default CRAN
rsession-ld-library-path=/opt/local/lib:/opt/local/someapp/lib
# Specifying R Version
rsession-which-r=/usr/local/bin/R
```  
2-2- Session configuration
```sh
# Create session configuration file
nano /etc/rstudio/rsession.conf
```
> Add the following code
```sh
# To limit the users who can login to RStudio to the members of a specific group, you use the auth-required-user-group setting. For example:
auth-required-user-group=rstudio_users
# Set session timeout
session-timeout-minutes=30
# set CRAN 
r-cran-repos=https://mirrors.nics.utk.edu/cran/
```

II - Ubuntu

> 0- Install R

```sh
# Step 1 — Setting Up APT
sudo sh -c 'echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list'
gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9
gpg -a --export E084DAB9 | sudo apt-key add -

# Install R
sudo apt-get update
sudo apt-get -y install r-base

```

> 1- Download and install R Studio

```sh
sudo apt-get install gdebi-core
wget https://download2.rstudio.org/rstudio-server-1.0.136-amd64.deb
sudo gdebi rstudio-server-1.0.136-amd64.deb

```

> 2- Configuring the Server
RStudio is configured by adding entries to two configuration files:
- /etc/rstudio/rserver.conf
- /etc/rstudio/rsession.conf
- /etc/pam.d/rstudio

```sh
# Create server configuration file
sudo nano /etc/rstudio/rserver.conf
```
> Add the following code
2-1- Server configuration
```sh
# RStudio port
www-port=8787
# Allow trafic from a specific IP
www-address=0.0.0.0 # world wide

```  
2-2- Session configuration
```sh
# Create session configuration file
sudo nano /etc/rstudio/rsession.conf
```

> Add the following code

```sh
# To limit the users who can login to RStudio to the members of a specific group, you use the auth-required-user-group setting. For example:
# set CRAN 
r-cran-repos=https://mirrors.nics.utk.edu/cran/
```

2-3- User authentication
```sh
sudo nano /etc/pam.d/rstudio
```

> Add the following code

```sh
@include common-auth
@include common-account
@include common-password
@include common-session
```

> Check configuration

```sh
sudo rstudio-server verify-installation
```


> 3- Managing server

```sh
# Stop
sudo rstudio-server stop
# Start
sudo rstudio-server start
# Restart
sudo rstudio-server restart
```



