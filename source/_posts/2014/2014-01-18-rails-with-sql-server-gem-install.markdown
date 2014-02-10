---
layout: post
title: "Rails with SQL Server gem install"
date: 2014-01-18T16:01:58+08:00
comments: true
external-url:
categories: [Rails, SQLServer]
---

Ubuntu 12.04 LTS already have the [freetds](https://launchpad.net/ubuntu/precise/+source/freetds) package, so just running below command will install [SQL Server support for Rails](https://github.com/rails-sqlserver/activerecord-sqlserver-adapter/wiki/Using-TinyTDS).

```bash install tiny_tds
apt-get install freetds-dev
gem install tiny_tds
gem install activerecord-sqlserver-adapter
```

```bash add below line to odbcinst.ini
#/etc/odbcinst.ini
[FreeTDS]
Description = ODBC for Microsoft SQL
Driver      = /usr/lib/i386-linux-gnu/odbc/libtdsodbc.so
UsageCount  = 1
Threading   = 2
```

About the driver location, you may need to find the libtdsodbc first at via `find /usr/lib -name libtdsodbc.so`.

```bash add below line to odbc.ini
#/etc/odbc.ini
[FreeTDS-TECN]
Description     = MS SQL connection to 'SDSS' database
Driver          = FreeTDS
Database        = TECN
ServerName      = CVPALPIP01
Trace           = No
```

```bash add below line to freetds.conf
#/etc/freetds/freetds.conf
[CVPALPIP01]
        host = 10.70.2.77
        port = 1433
        tds version = 7.0 # or 8.0
```

```yml add below line to database.yml
tecn:
  adapter: sqlserver
  dataserver: '10.70.2.77\SDSS'
  database: TECN
  username: only_read
  password: only_read
```

```rb and using it in model
# app/models/tecn.rb
class Tecn < ActiveRecord::Base
  # No corresponding table in the DB.
  self.abstract_class = true
  establish_connection("tecn")
  def readonly?
  	true
  end
end
```