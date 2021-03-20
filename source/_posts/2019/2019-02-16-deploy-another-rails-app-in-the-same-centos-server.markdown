---
layout: post
title: "Deploy another Rails app in the same CentOS server"
date: 2019-02-16T20:40:28+08:00
comments: true
external-url:
categories: [CentOS]
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
git clone https://github.com/andorchen/rbenv-china-mirror.git "$(rbenv root)"/plugins/rbenv-china-mirror
```

# Install Ruby 2.7.1

```bash
rbenv install -l
rbenv install 2.7.1
rbenv global 2.7.1
rbenv shell 2.7.1
eval "$(rbenv init -)" >> ~/.bash_profile
echo "gem: --no-document" > ~/.gemrc
gem install bundler --default -v "1.17.3"
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

# Create mysql DB

```
CREATE USER 'cybros_staging'@'localhost' IDENTIFIED BY 'password';
CREATE DATABASE cybros_staging character set UTF8mb4 collate utf8mb4_bin;
GRANT ALL PRIVILEGES ON cybros_staging.* to 'cybros_staging'@'localhost';
FLUSH PRIVILEGES;
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
ALTER TABLE schema_migrations OWNER TO sccsa_users;
ALTER TABLE ar_internal_metadata OWNER TO sccsa_users;
ALTER TABLE active_storage_attachments OWNER TO sccsa_users;
ALTER TABLE active_storage_blobs OWNER TO sccsa_users;
ALTER TABLE active_storage_variant_records OWNER TO sccsa_users;
ALTER SEQUENCE wechat_sessions_id_seq OWNER TO sccsa_users;
```


# SQL which may help to build above ALTER faster.

```sql
SELECT 'ALTER TABLE '||table_name||' OWNER TO sccsa_users;'
FROM information_schema.tables
WHERE table_schema = 'public'
ORDER BY table_name;
SELECT 'ALTER SEQUENCE '||sequence_name||' OWNER TO sccsa_users;'
FROM information_schema."sequences"
WHERE sequence_schema = 'public';
ORDER BY sequence_name;
```

[Further reference](https://www.digitalocean.com/community/tutorials/how-to-use-roles-and-manage-grant-permissions-in-postgresql-on-a-vps--2).
