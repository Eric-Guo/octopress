---
layout: post
title: "Install Shiny Server in Ubuntu 12.04"
date: 2013-01-24T22:09:16+08:00
comments: true
external-url:
categories: [Node.js,R]
---
R package [Shiny](http://cran.r-project.org/web/packages/shiny/index.html) originally not support R Studio Server, so [shiny.R](https://github.com/rstudio/shiny/raw/master/R/shiny.R) need change before install

```bash install shiny package for R Studio
cd ~
wget http://cran.r-project.org/src/contrib/shiny_0.11.1.tar.gz
tar xvfz shiny_0.11.1.tar.gz
vi shiny/R/server.R
# search 'Listening on http://' and replace 'startServer(host, port' to 'startServer("0.0.0.0", port'
# search 'Test port to see if we can use it' and replace 'try(startServer(host, port' to 'try(startServer("0.0.0.0", port'
# search 'QtWebKit' and replace 'paste("http://", browseHost,' to 'paste("http://", "your.rstudio.host.name",'
tar cvfz shiny_0.11.1-server.tar.gz shiny
R CMD INSTALL shiny_0.11.1-server.tar.gz (may need install htmltools before this line)
service rstudio-server restart # if you previous install shiny from CRAN
```

Before install shiny server, make sure you install node.js version 0.8.17 or higher and meet [prerequisites](https://github.com/rstudio/shiny-server#prerequisites).

```bash install shiny-server
npm install -g shiny-server # install shiny-server
```

or use the offline install mode:

```bash
git clone https://github.com/rstudio/shiny-server
cd shiny-server/
node tools/bundle-offline-installer.js
mv shiny-server-0.3.0.tgz ..
cd ..
npm install --no-registry -g shiny-server-0.3.0.tgz
```

The sample shiny server config file.

```nginx
run_as shiny;
log_dir /var/log/shiny-server/;
server {
  listen 3838 127.0.0.1;

  location /first {
    app_dir /var/shiny-server/first;
  }

  location /diamonds {
    app_dir /var/shiny-server/diamonds;
  }
}
```