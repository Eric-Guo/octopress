---
layout: post
title: "Installation log on Ubuntu 14.04 LTS on vSphere 5.5"
date: 2015-08-28T05:50:11+08:00
comments: true
external-url: 
categories: [Linux]
---


### Install SSH


```bash Upgrade Ubuntu and enable SSH server
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install open-vm-tools
sudo apt-get install openssh-server
```


```bash Install JDK
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update && sudo apt-get install oracle-java7-installer
```


```bash Install rvm
sudo su - # switch to root if not yet
apt-get install curl
curl -L get.rvm.io | bash -s master # install rvm
logout
sudo su -
rvm requirements # to install all depends
```


```bash Install Ruby
rvm install ruby-2.2.3
gem source --remove https://rubygems.org/
gem source --add https://ruby.taobao.org/
gem update --system
apt-get install libgmp3-dev
echo "gem: --no-document" >> /etc/gemrc
gem update
gem install bundler
```


```bash Install Oracle Client
apt-get install unrar
apt-get install libaio-dev # must need if you want to use sqlplus
mkdir -p /opt/oracle && cd /opt/oracle
unrar x /home/eric/instantclient-all-linux.x64-11.2.0.4.0.rar .
chmod +x instantclient_11_2/sqlplus
ln -s /opt/oracle/instantclient_11_2/libclntsh.so.11.1 /opt/oracle/instantclient_11_2/libclntsh.so
ln -s /opt/oracle/instantclient_11_2/sqlplus /usr/local/bin/sqlplus
echo "/opt/oracle/instantclient_11_2" >> /etc/ld.so.conf.d/oracle_instantclient.conf
```


```text
# append below line to /etc/enviroment file
ORACLE_BASE=/opt/oracle
ORACLE_HOME=/opt/oracle/instantclient_11_2
TNS_ADMIN=/opt/oracle/network/admin
LD_LIBRARY_PATH=/opt/oracle/instantclient_11_2
OCI_HOME=/opt/oracle/instantclient_11_2
OCI_LIB=/opt/oracle/instantclient_11_2
OCI_LIB_DIR=/opt/oracle/instantclient_11_2
OCI_INCLUDE_DIR=/opt/oracle/instantclient_11_2/sdk/include
NLS_LANG=AMERICAN_AMERICA.AL32UTF8
rvmsudo_secure_path=1
```


```bash update the library and install ruby-oci8
reboot
sudo su -
ldconfig
mkdir -p network/admin
mv /home/eric/tnsnames.ora .
gem install ruby-oci8 # should be no any error here :-)
```


```text add R source
sudo vi /etc/apt/sources.list
# append below line to end of sources.list
# you can view mirror at http://cran.r-project.org/mirrors.html
deb http://cran.ism.ac.jp/bin/linux/ubuntu trusty/
```


```bash prepare to install R 
apt-get update
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 51716619E084DAB9
# replace your NO_PUBKEY error above, if pause/block port, using 80 as below line
gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E084DAB9
gpg -a --export E084DAB9 | sudo apt-key add -
apt-get update # make sure no error here
```


```bash install R
apt-get install r-base
```


```bash manual install the ROracle
wget http://cran.r-project.org/src/contrib/DBI_0.3.1.tar.gz
R CMD INSTALL DBI_0.3.1.tar.gz
wget http://cran.r-project.org/src/contrib/ROracle_1.2-1.tar.gz
R CMD INSTALL --configure-args='--with-oci-inc=/opt/oracle/instantclient_11_2/sdk/include --with-oci-lib=/opt/oracle/instantclient_11_2' ROracle_1.2-1.tar.gz
```
