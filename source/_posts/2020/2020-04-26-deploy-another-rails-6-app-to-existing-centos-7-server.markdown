---
layout: post
title: "Deploy another rails 6 app to existing CentOS 7 server"
date: 2020-04-26T08:32:59+08:00
comments: true
external-url:
categories: [Rails, Ruby]
---

## Prepare the execution files & account

```bash
adduser sccsa_web
sudo su - sccsa_web
mkdir .ssh
chmod 700 .ssh
vi .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
```

# Install rbenv and ruby-build

```bash
cd # as a sccsa_web
git clone https://github.com/sstephenson/rbenv.git .rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
~/.rbenv/bin/rbenv init
# As an rbenv plugin
mkdir -p "$(rbenv root)"/plugins
# git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
git clone https://gitee.com/Eric-Guo/ruby-build.git "$(rbenv root)"/plugins/ruby-build
# git clone https://github.com/andorchen/rbenv-china-mirror.git "$(rbenv root)"/plugins/rbenv-china-mirror
git clone https://gitee.com/Eric-Guo/rbenv-china-mirror.git "$(rbenv root)"/plugins/rbenv-china-mirror
```

# Install Ruby 3.0.1

```bash
rbenv install -l
rbenv install 3.0.1
rbenv global 3.0.1
rbenv shell 3.0.1
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
echo "gem: --no-document" > ~/.gemrc
# gem install bundler --default -v "1.17.3"
gem install bundler
bundle config build.pg --with-pg-config=/usr/pgsql-13/bin/pg_config
```

# Fix permission for CentOS

```bash
sudo mkdir /var/www
cd /var/www
sudo mkdir sccsa_web
sudo chown sccsa_web:sccsa_web sccsa_web/
```


