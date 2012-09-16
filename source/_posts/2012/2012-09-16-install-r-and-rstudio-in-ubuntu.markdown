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

##### Install r-base
```bash
apt-get install r-base
```

##### Install Oracle DB access package
You can found new version of [ROracle](http://cran.r-project.org/web/packages/ROracle/index.html) or [DBI](http://cran.r-project.org/web/packages/DBI/index.html) package in [CRAN](http://cran.r-project.org/index.html), it is also required you [properly install the Oracle Instant Client](/2012/08/13/another-install-phusion-passenger-and-nginx-log/).

```bash
wget http://cran.r-project.org/src/contrib/DBI_0.2-5.tar.gz
R CMD INSTALL DBI_0.2-5.tar.gz
wget http://cran.r-project.org/src/contrib/ROracle_1.1-4.tar.gz
R CMD INSTALL --configure-args='--with-oci-inc=/opt/oracle/instantclient_11_2/sdk/include' ROracle_1.1-4.tar.gz
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
groupadd rstudio
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