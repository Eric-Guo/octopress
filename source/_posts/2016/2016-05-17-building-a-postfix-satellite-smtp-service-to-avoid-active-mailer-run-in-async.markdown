---
layout: post
title: "Building a Postfix Satellite SMTP service to avoid Active Mailer run in Aliyun"
date: 2016-05-17T14:17:02+08:00
comments: true
external-url: 
categories: [CentOS, Rails]
---

Some situation is better avoid Sidekiq like reporting the exception mail, so it's always the best practices to building a Postfix Satellite SMTP service instead.

# Install Postfix

```bash
yum install -y postfix cyrus-sasl-plain # cyrus-sasl-plain is no need for Ubuntu
yum erase -y sendmail* # In case Sendmail is installed
```

# Configure

Similar to [gmail](http://mhawthorne.net/posts/postfix-configuring-gmail-as-relay.html) or [SendGrid](https://www.linode.com/docs/email/postfix/postfix-smtp-debian7), every mail service vendor offer different combination of setting, below is only apply to [AliYun](https://qiye.aliyun.com/)

## /etc/postfix/main.cf change

```text
# comment out below
#inet_interfaces = localhost
relayhost = [smtp.mxhichina.com]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_CApath = /etc/ssl/certs
smtp_use_tls = yes
smtp_generic_maps = hash:/etc/postfix/generic
mynetworks = 127.0.0.0/8
mydestination =
```

## Create /etc/postfix/sasl_passwd 

```
[smtp.mxhichina.com]:587    no-reply@domain.com:PASSWORD
```

## Create /etc/postfix/generic 

```
root@staging.domain.com   no-reply@domain.com
```

## Create a database file

```bash
postmap /etc/postfix/sasl_passwd /etc/postfix/generic
```

## Prevent non-root access:

```bash
chmod 0600 /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db /etc/postfix/generic /etc/postfix/generic.db
```

## Restart Postfix service

```bash
/etc/init.d/postfix restart # or service postfix restart
```

# Debug

It's very common that postfix will failed to sent mail at first time, so add below to main.cf.

```text
debug_peer_level = 3
debug_peer_list = smtp.mxhichina.com
```

And open another console to testing via:

```bash
echo "body of your email" | mail -s 'mail subject from console' -r 'no-reply@domain.com' guochunzhong@domain.com
```

You may also need `postconf -n` to review postfix configuration or `postconf -d` to review all setting.

YOu can also monitor the postfix activity via `tail -f /var/log/syslog`.
