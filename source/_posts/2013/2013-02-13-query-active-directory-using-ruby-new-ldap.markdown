---
layout: post
title: "Query Active Directory using Ruby net ldap gems"
date: 2013-02-13T20:32:32+08:00
comments: true
external-url:
categories: [Ruby, Windows7, Linux]
---

I'm trying [devise_ldap_authenticatable](https://github.com/cschiewek/devise_ldap_authenticatable) during 2013 CNY, after spending a lot of time, found below script is quite usful if you want to using LDAP authenticate but found something wrong and start trying to debug.

<!--more-->

```bash Confirm first command line ldap works good
ldapsearch -x -LLL -D "SDCORP\mes.service" -w "Password" -b "DC=sdcorp,DC=global,DC=sandisk,DC=com" -s sub -H ldap://cvpcdcip04 "cn=Eric Guo" cn mail displayName samaccountname
```

```ruby Using ruby gems to test
require 'net/ldap'

ldap = Net::LDAP.new :host => 'cvpcdcip04',
     :port => 389,
     :auth => {
           :method => :simple,
           :username => "MES Service",
           :password => "Password"
     }

filter = Net::LDAP::Filter.eq("cn", "MES Service")
# treebase must exactly match, otherwise can not found the entity
treebase = "DC=sdcorp,DC=global,DC=sandisk,DC=com"

res=ldap.search(:base => treebase, :filter => filter)

p ldap.get_operation_result
```
