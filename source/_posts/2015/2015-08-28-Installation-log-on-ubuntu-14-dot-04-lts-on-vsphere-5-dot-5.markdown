---
layout: post
title: "Installation log on Ubuntu 14.04 LTS on vSphere 5.5"
date: 2015-08-28T05:50:11+08:00
comments: true
external-url: 
categories: [Linux]
---

My log to build a Ubuntu server to fit my software stack usage, R & Ruby.

#### Install SSH


```bash Upgrade Ubuntu and enable SSH server
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install open-vm-tools
sudo apt-get install openssh-server
```

#### Install JDK

```bash
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update && sudo apt-get install oracle-java7-installer
```

#### Install rvm

```bash 
sudo su - # switch to root if not yet
apt-get install curl
curl -L get.rvm.io | bash -s master # install rvm
logout
sudo su -
rvm requirements # to install all depends
```

#### Install Ruby

```bash 
rvm install ruby-2.2.3
gem source --remove https://rubygems.org/
gem source --add https://ruby.taobao.org/
gem update --system
apt-get install libgmp3-dev
echo "gem: --no-document" >> /etc/gemrc
gem update
gem install bundler
```

#### Install Oracle Client

```bash 
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

#### Install Oracle gems ruby-oci8

```bash update the library and install ruby-oci8
reboot
sudo su -
ldconfig
mkdir -p network/admin
mv /home/eric/tnsnames.ora .
gem install ruby-oci8 # should be no any error here :-)
```

#### Install R

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

#### Install ROracle

```bash manual install the ROracle
wget http://cran.r-project.org/src/contrib/DBI_0.3.1.tar.gz
R CMD INSTALL DBI_0.3.1.tar.gz
wget http://cran.r-project.org/src/contrib/ROracle_1.2-1.tar.gz
R CMD INSTALL --configure-args='--with-oci-inc=/opt/oracle/instantclient_11_2/sdk/include --with-oci-lib=/opt/oracle/instantclient_11_2' ROracle_1.2-1.tar.gz
```

#### Install Passenger

Just following Passenger [documentation](https://www.phusionpassenger.com/library/install/nginx/install/oss/trusty/)

```bash install phusionpassenger
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo apt-get install -y apt-transport-https ca-certificates

sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger trusty main > /etc/apt/sources.list.d/passenger.list'
sudo apt-get update

sudo apt-get install -y nginx-extras passenger
```

```text vi /etc/nginx/nginx.conf
passenger_root /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini;
passenger_ruby /usr/local/rvm/gems/ruby-2.2.3/wrappers/ruby;
passenger_max_pool_size 30;
```

#### Install R studio server

[Install R studio server](https://www.rstudio.com/products/rstudio/download-server/)

```bash
sudo apt-get install gdebi-core
wget https://download2.rstudio.org/rstudio-server-0.99.473-amd64.deb
sudo gdebi rstudio-server-0.99.473-amd64.deb
```

```text vi /etc/rstudio/rserver.conf
# Server Configuration File
www-address=127.0.0.1
```

```text vi /etc/nginx/sites-available/cvprstudio 
server {
  listen       80;
  server_name  cvprstudio;
  server_name  cvprstudio.sandisk.com;
  client_max_body_size 10m;
  location / {
    proxy_pass http://127.0.0.1:8787;
    proxy_redirect http://127.0.0.1:8787/ $scheme://$host/;
  }
}
```

```bash
cd /etc/nginx/sites-enabled/
ln -s /etc/nginx/sites-available/cvprstudio cvprstudio
service nginx restart
```

#### Install RStudio Shiny Server

[Install RStudio Shiny Server](https://www.rstudio.com/products/shiny/download-server/)

```bash run as root
R -e "install.packages('shiny', repos='https://cran.rstudio.com/')"
wget https://download3.rstudio.org/ubuntu-12.04/x86_64/shiny-server-1.4.0.721-amd64.deb
gdebi shiny-server-1.4.0.721-amd64.deb
```

```text vi /etc/nginx/sites-available/shinyapp 
server {
  listen       80;
  server_name  shinyapp;
  server_name  shinyapp.sandisk.com;
  location / {
    proxy_pass http://127.0.0.1:3838;
    proxy_redirect http://127.0.0.1:3838/ $scheme://$host/;
  }
}
```

```bash
cd /etc/nginx/sites-enabled/
ln -s /etc/nginx/sites-available/shinyapp shinyapp
service nginx restart
```

Following [patch process in previous blog](/2013/01/24/install-shiny-server-in-ubuntu-12-dot-04/) to re-install the shiny

```bash
R CMD INSTALL shiny_0.12.2-server.tar.gz 
```

