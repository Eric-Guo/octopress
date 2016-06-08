---
layout: post
title: "Monitor Sidekiq service with Monit &amp; Capistrano"
date: 2016-06-08T21:13:32+08:00
comments: true
external-url: 
categories: [CentOS, Ruby]
---

[Sidekiq](http://sidekiq.org) as a background processing facility in Rails, usually more and more important during business grow, but it's relative quite possible failed to startup due to there is some code error, so here is my solution to monitor it.

I choose the [monit instead of god](http://stackoverflow.com/questions/768184/god-vs-monit) beause monit is written in C and very small memory footprint. The [capistrano-sidekiq](https://github.com/seuros/capistrano-sidekiq#customizing-the-monit-sidekiq-templates) also having a out of box support, but when you combine with monit, capistrano, sidekiq it still trick, so I feel writing a blog is worth.

## Install monit

```bash
yum install monit # assure using CentOS
```

## Configure monit

`vi /etc/monitrc`:

```text
set daemon  60

set mailserver localhost

set mail-format {
     from: monit@your-domain.com
  subject: monit alert --  $EVENT $SERVICE
  message: $EVENT Service $SERVICE

Hi all people emails address;

Event happen at: $DATE
Action:          $ACTION
Host:            $HOST
Description: $DESCRIPTION

Your faithful employee,

Monit (robot)
Hint: when resolved, please replay all.
}

set alert receiver1@mail.com
set alert receiver2@mail.com
```

## Configure in [capistrano-sidekiq](https://github.com/seuros/capistrano-sidekiq)

At `config/deploy/production.rb`

```
set :sidekiq_monit_conf_dir, '/etc/monit.d' # for CentOS folder
set :sidekiq_monit_use_sudo, false
```

At `Capfile`

```
require 'capistrano/sidekiq'
require 'capistrano/sidekiq/monit' # to require monit tasks
```

## Enable alert by customize the monit template.

```bash
rails g capistrano:sidekiq:monit:template
```

and change as below:

```erb
# Monit configuration for Sidekiq :  <%= fetch(:application) %>
<% processes_pids.each_with_index do |pid_file, idx| %>
check process <%= sidekiq_service_name(idx) %> with pidfile "<%= pid_file %>"
  if changed pid then alert
  start program = "/bin/su - <%= @role.user %> -c 'cd <%= current_path %> && <%= SSHKit.config.command_map[:sidekiq] %> <%= sidekiq_config %> --index <%= idx %> --pidfile <%= pid_file %> --environment <%= fetch(:sidekiq_env) %> <%= sidekiq_concurrency %> <%= sidekiq_logfile %> <%= sidekiq_queues %> <%= sidekiq_options_per_process[idx] %> -d'" with timeout 30 seconds

  stop program = "/bin/su - <%= @role.user %> -c 'cd <%= current_path %> && <%= SSHKit.config.command_map[:sidekiqctl] %> stop <%= pid_file %>'" with timeout <%= fetch(:sidekiq_timeout).to_i + 10  %> seconds
  group <%= fetch(:sidekiq_monit_group, fetch(:application)) %>-sidekiq

<% end %>
```

## Deploya to generate minit config

```
cap production deploy
```

After that checking if `/etc/monit.d` having sidekiq conf file.

## Running monit at production

```bash
monit
```

To test the mail, using `monit reload`, if still can not received the mail, checking [my postfix blog](/2016/05/17/building-a-postfix-satellite-smtp-service-to-avoid-active-mailer-run-in-async/).
