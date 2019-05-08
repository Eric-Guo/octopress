---
layout: post
title: "Deploy another Rails app in the same CentOS server"
date: 2019-02-16T20:40:28+08:00
comments: true
external-url:
categories:
---

Assure the first Rails app is running as user `deployer` and second as user `scschub`.

# Setup second user account

```bash
adduser scschub
gpasswd -a scschub wheel
visudo # add scschub ALL=(ALL) NOPASSWD: ALL at end
sudo su - scschub
mkdir .ssh
chmod 700 .ssh
```

# Install rbenv and ruby-build

```bash
cd # as a deployer
git clone git://github.com/sstephenson/rbenv.git .rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
~/.rbenv/bin/rbenv init
# As an rbenv plugin
mkdir -p "$(rbenv root)"/plugins
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
```

# Install Ruby 2.6.3

```bash
rbenv install -l
rbenv install 2.6.3
rbenv global 2.6.3
eval "$(rbenv init -)" >> ~/.bash_profile
echo "gem: --no-document" > ~/.gemrc
gem install bundler
```

# Fix permission for CentOS

```bash
sudo mkdir /var/www
cd /var/www
sudo mkdir scschub
sudo chown scschub:scschub scschub/
```

# Copy puma config.rb and other shared link files

```
cap production puma:config
```

# Create postgresql role


```bash
sudo su - postgres
createuser scschub --pwprompt
psql
ALTER ROLE scschub LOGIN
CREATE ROLE sccsa_users;
GRANT sccsa_users TO deployer;
GRANT sccsa_users TO scschub;
ALTER ROLE deployer INHERIT;
ALTER ROLE scschub INHERIT;
```

# Allow both user can access the same data.

```
psql -d sccsa_production
ALTER TABLE wechat_sessions OWNER TO sccsa_users;
ALTER SEQUENCE wechat_sessions_id_seq OWNER TO sccsa_users;
```

[Further reference](https://www.digitalocean.com/community/tutorials/how-to-use-roles-and-manage-grant-permissions-in-postgresql-on-a-vps--2).
