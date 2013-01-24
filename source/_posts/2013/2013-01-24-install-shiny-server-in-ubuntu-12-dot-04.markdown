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
wget http://cran.r-project.org/src/contrib/shiny_0.3.0.tar.gz
tar xvfz shiny_0.3.0.tar.gz
vi shiny/R/shiny.R # search localhost and replace to your rstudio host name
tar cvf shiny_0.3.0-server.tar.gz shiny
R CMD INSTALL shiny_0.3.0-server.tar.gz
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