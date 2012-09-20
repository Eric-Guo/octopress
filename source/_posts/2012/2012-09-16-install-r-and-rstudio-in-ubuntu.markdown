---
layout: post
title: "Install R & RStudio in Ubuntu"
date: 2012-09-16 10:46
comments: true
external-url:
categories: [R]
---
Install R in Ubuntu is extremely easy if you don't meet any exception, but if you meet, then you'd better be a very advanced linux user :-)
<!--more-->

##### Install R
Because the Ubuntu official source R version is usually half of years older than R-project official source, so it is recommanded to using [r-project.org official source](http://cran.r-project.org/bin/linux/ubuntu/README) to install the latest R system.

```text vi /etc/apt/sources.list
# append below line to end of sources.list
# you can view mirror at http://cran.r-project.org/mirrors.html
deb http://ftp.ctex.org/mirrors/CRAN/bin/linux/ubuntu precise/
```

```bash import the GPG key and install r-base
cd ~
gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E084DAB9
gpg -a --export E084DAB9 | sudo apt-key add -
apt-get upgrade
apt-get install r-base
```

##### Install Oracle DB access package
You can found new version of [ROracle](http://cran.r-project.org/web/packages/ROracle/index.html) or [DBI](http://cran.r-project.org/web/packages/DBI/index.html) package in [CRAN](http://cran.r-project.org/index.html), it is also required you [properly install the Oracle Instant Client](/2012/08/13/another-install-phusion-passenger-and-nginx-log/).

```bash manual install the ROracle
wget http://cran.r-project.org/src/contrib/DBI_0.2-5.tar.gz
R CMD INSTALL DBI_0.2-5.tar.gz
wget http://cran.r-project.org/src/contrib/ROracle_1.1-5.tar.gz
R CMD INSTALL --configure-args='--with-oci-inc=/opt/oracle/instantclient_11_2/sdk/include' ROracle_1.1-5.tar.gz
```

##### Install [RStudio Server](http://www.rstudio.org/download/server)

```bash
apt-get install libssl0.9.8 # must install even you have newer version
apt-get install libapparmor1 apparmor-utils
wget http://download2.rstudio.org/rstudio-server-0.96.331-i386.deb
dpkg -i rstudio-server-0.96.331-i386.deb
rstudio-server verify-installation
```

##### Do some RStudio Server setting

```bash
echo 'rsession-memory-limit-mb=1000' > /etc/rstudio/rserver.conf
echo 'rsession-stack-limit-mb=4' >> /etc/rstudio/rserver.conf
echo 'rsession-process-limit=20' >> /etc/rstudio/rserver.conf
# Only pass below if you will using proxy mode
echo 'www-address=127.0.0.1' >> /etc/rstudio/rserver.conf
groupadd rstudio
```

##### Setting the proxy server for RStudio server

This section is optional, assured already [install nginx](/2012/08/13/another-install-phusion-passenger-and-nginx-log/) in server.

```nginx do not forgot link to /opt/nginx/conf/vhosts
server {
  listen       80;
  server_name  cvprstudio;
  location / {
    proxy_pass http://localhost:8787;
    proxy_redirect http://localhost:8787/ $scheme://$host/;
  }
}
```

##### Setting auto restart and PATH

```bash
ln -s /usr/lib/rstudio-server/extras/init.d/debian/rstudio-server /etc/init.d/rstudio-server
vi /etc/init.d/rstudio-server
```

```text append below line to /etc/init.d/rstudio-server SCRIPTNAME
ORACLE_BASE=/opt/oracle
ORACLE_HOME=/opt/oracle/instantclient_11_2
TNS_ADMIN=/opt/oracle/network/admin
NLS_LANG=AMERICAN_AMERICA.AL32UTF8
```

```bash Now you can restart/start via standard init.d service way
/etc/init.d/rstudio-server restart
```

##### Add a user in RStudio

```bash
adduser --ingroup rstudio cindy
passwd cindy # setting password
```

##### Update package
Usually it is more good to upgrade the r-base in system wide packages instead of per user.

```rconsole after run R in root console
update.packages() # select mirror to check
```