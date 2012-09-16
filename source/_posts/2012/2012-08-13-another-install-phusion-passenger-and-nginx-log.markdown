---
layout: post
title: "Another Install Phusion Passenger &amp; nginx log"
date: 2012-08-13 11:00
comments: true
external-url:
categories: [Rails, Linux]
---

There is tons of good installation guide for Rails, but I still think it's reasonable to log my installation here as I will change it time to time to make this blog ever better.<!--more-->

## Part 1, Install Common Rails Passenger environment

```bash Upgrade Ubuntu 12.04 and enable SSH server
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install openssh-server
```

```bash Test SSH and add authorized_keys
mkdir -p ~/.ssh
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAsBsQ623fdllkyWBnmIVL9lfaxLGKulWv7jwhxLIhp+3fFZ/K0yGeUoJig6a4UlFFmIPibWiubP7utqvJOZO4psdX+GM42HCU/JZmG3gJqYRgwUMupeb8BXEL4r/nG/YE84kOYh1jF8yjNHs2ZSeaO4M4v7Mcsi8USWTM4mTZhO9HZcQZCQBZbkzCf2PwgNq8q7G9jXq6kU+mpUCbwVFZF1R461lzFOIjZQUvxZ+ylKdZxyX0AGCdLybSPcJJkE/H5FMs5KnInme5/692uz8DjXHi0ddw8s6bfIJI7a9av58kJlZNFu/XDWF7WfoNVRhQWn+cl0eBO+hRlUxMCM8jTw== Eric Guo' > ~/.ssh/authorized_keys
```

```bash Install rvm in root
sudo su # switch to root if not yet
apt-get install curl
curl -L get.rvm.io | bash -s stable # install rvm
logout # suggest logout and enter again to test rvm works
```

```bash Install rvm requirement package
rvm requirements
```

Read comment output request and copy paste below command:

    Additional Dependencies:
    # For Ruby / Ruby HEAD (MRI, Rubinius, & REE), install the following:
      ruby: /usr/bin/apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion


```bash Install rvm dependency packages
apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion # do not copy this line, copy your rvm requirements output
```

```bash Install MRI Rubies and Rails
rvm list known # find your favorate ruby version
rvm install 1.9.3-p194 --patch railsexpress # here is lastest stable version
ruby -v # to verify it is install right
gem update --system # upgrade gem package
gem install rails # install latest stable rails
```

```bash Install Phusion Passenger and nginx
nginx -v # make sure no nginx install, if install need remove by running gem install passenger
gem install passenger
passenger-install-nginx-module # running step by step installer
apt-get install libcurl4-openssl-dev # install missing package
passenger-install-nginx-module # rerun and enter until you meed below notice
```

    Automatically download and install Nginx?

    Nginx doesn't support loadable modules such as some other web servers do,
    so in order to install Nginx with Passenger support, it must be recompiled.

    Do you want this installer to download, compile and install Nginx for you?

     1. Yes: download, compile and install Nginx for me. (recommended)
        The easiest way to get started. A stock Nginx 1.2.2 with Passenger
        support, but with no other additional third party modules, will be
        installed for you to a directory of your choice.

     2. No: I want to customize my Nginx installation. (for advanced users)
        Choose this if you want to compile Nginx with more third party modules
        besides Passenger, or if you need to pass additional options to Nginx's
        'configure' script. This installer will  1) ask you for the location of
        the Nginx source code,  2) run the 'configure' script according to your
        instructions, and  3) run 'make install'.

    Whichever you choose, if you already have an existing Nginx configuration file,
    then it will be preserved.

Answer `1`, install from fresh and accept default install location `/opt/nginx`

If you see below, you successfully install nginx and Passenger:

    Deploying a Ruby on Rails application: an example

    Suppose you have a Ruby on Rails application in /somewhere. Add a server block
    to your Nginx configuration file, set its root to /somewhere/public, and set
    'passenger_enabled on', like this:

       server {
          listen 80;
          server_name www.yourhost.com;
          root /somewhere/public;   # <--- be sure to point to 'public'!
          passenger_enabled on;
       }

    And that's it! You may also want to check the Users Guide for security and
    optimization tips and other useful information:

      /usr/local/rvm/gems/ruby-1.9.3-p194/gems/passenger-3.0.15/doc/Users guide Nginx.html

    Enjoy Phusion Passenger, a product of Phusion (www.phusion.nl) :-)
    https://www.phusionpassenger.com

    Phusion Passenger is a trademark of Hongli Lai & Ninh Bui.

```bash Install nginx init script
cd
git clone https://github.com/jnstq/rails-nginx-passenger-ubuntu.git
sudo mv rails-nginx-passenger-ubuntu/nginx/nginx /etc/init.d/nginx
sudo chown root:root /etc/init.d/nginx
```

```bash Configure nginx to enable vhosts (site by site configure file)
logout # as root now
mkdir -p include /opt/nginx/conf/vhosts/
chmod 755 /opt/nginx/conf/vhosts/
vi /opt/nginx/conf/nginx.conf # append 'include /opt/nginx/conf/vhosts/*;' before last }
```

You may also want to set some [Passenger parameter](http://www.modrails.com/documentation/Users%20guide%20Nginx.html#_configuring_phusion_passenger) like `passenger_max_pool_size 15;` according to your system status.

```bash Create rails root folder
sudo mkdir -p /var/rails_apps
sudo chmod 777 /var/rails_apps/ #giving full file permissions
```

## Part 2, Install one application to site

```bash Create per application user (using pl-form as example)
sudo adduser \
  --system \
  --shell /bin/bash \
  --gecos 'eForms' \
  --group \
  --home /home/pl-form pl-form
passwd pl-form # setting application password
```

```bash Prepare application access and private key
sudo su - pl-form
mkdir -p ~/.ssh
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAsBsQ623fdllkyWBnmIVL9lfaxLGKulWv7jwhxLIhp+3fFZ/K0yGeUoJig6a4UlFFmIPibWiubP7utqvJOZO4psdX+GM42HCU/JZmG3gJqYRgwUMupeb8BXEL4r/nG/YE84kOYh1jF8yjNHs2ZSeaO4M4v7Mcsi8USWTM4mTZhO9HZcQZCQBZbkzCf2PwgNq8q7G9jXq6kU+mpUCbwVFZF1R461lzFOIjZQUvxZ+ylKdZxyX0AGCdLybSPcJJkE/H5FMs5KnInme5/692uz8DjXHi0ddw8s6bfIJI7a9av58kJlZNFu/XDWF7WfoNVRhQWn+cl0eBO+hRlUxMCM8jTw== Eric Guo' > ~/.ssh/authorized_keys
vi ~/.ssh/id_rsa # copy & paste your key here
chmod 600 ~/.ssh/id_rsa
```

```bash Clone the application source code from git
sudo su - pl-form
cd /var/rails_apps
git clone gitolite@cvpscmip01:pl-form.git
```

```bash Install application depend gems
logout # as root now
cd /var/rails_apps/pl-form
bundle # install system gems
```

```bash Bundle and Migrate database
sudo su - pl-form
cd /var/rails_apps/pl-form
bundle
rake db:migrate # migrate db
```

```bash Append application conf to nginx server
logout # as root now
mkdir -p ~/confs/nginx
cd ~/confs/nginx
vi cvpforms # using below nginx conf as example
```

```nginx Sample pl-form nginx configure file
server {
  listen       80;
  server_name  cvpforms; # replace your DNS name
  rails_env    development; # development or production
  root         /var/rails_apps/pl-form/public;
  passenger_enabled on;
}
```

```bash Link the configure file and restart nginx
cd /opt/nginx/conf/vhosts
ln -s ~/confs/nginx/cvpforms cvpforms
cd /var/rails_apps/pl-form
touch tmp/restart.txt # or restart the whole server /etc/init.d/nginx restart
```

## Part 3 Install Oracle Client

Download only instantclient-basic, instantclient-sqlplus and instantclient-sdk zip archives and unzip them all into the same instantclient_11_2 folder. [x86](http://www.oracle.com/technetwork/topics/linuxsoft-082809.html) [x64](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html)

```bash Install Oracle Instant Client
sudo apt-get install libaio-dev # must need if you want to use sqlplus
pscp -p -r instantclient_11_2 root@cvprcsip01:/opt/oracle/ # copy extraced oracle instant client
ln -s /opt/oracle/instantclient_11_2/libclntsh.so.11.1 /opt/oracle/instantclient_11_2/libclntsh.so
ln -s /opt/oracle/instantclient_11_2/sqlplus /usr/local/bin/sqlplus
```

```text append below line to /etc/enviroment file
ORACLE_BASE=/opt/oracle
ORACLE_HOME=/opt/oracle/instantclient_11_2
TNS_ADMIN=/opt/oracle/network/admin
LD_LIBRARY_PATH=/opt/oracle/instantclient_11_2
NLS_LANG=AMERICAN_AMERICA.AL32UTF8
```

```text also add to /etc/ld.so.conf
include /etc/ld.so.conf.d/*.conf
/opt/oracle/instantclient_11_2
```

```bash update the library
ldconfig
```

Do not forget to config `tnsnames.ora` at `/opt/oracle/network/admin` and running `sqlplus` to ensure oracle client can link to the database correctly.

```bash install ruby-oci8 gems now
gem install ruby-oci8 # should be no any error here :-)
```

## End of beginning your rails way

Setting up the production rails environment is straight forward and it's just [the end of beginning Ruby on Rails](http://www.jasimabasheer.com/posts/meta_introduction_to_ruby.html).