---
layout: post
title: "Frequently used R package list and install hint"
date: 2013-05-24T21:51:21+08:00
comments: true
external-url:
categories: [R]
---

Confirm works at Ubuntu 14.04 / 12.04.

* [knitr](http://cran.r-project.org/web/packages/knitr/) - A general-purpose package for dynamic report generation in R
* [rmarkdown](http://cran.r-project.org/web/packages/rmarkdown/) - Dynamic Documents for R
* [SixSigma](http://cran.r-project.org/web/packages/SixSigma/) - Six Sigma Tools for Quality and Process Improvement
* [sqldf](http://cran.r-project.org/web/packages/sqldf/) - Perform SQL Selects on R Data Frames
* [tseries](http://cran.r-project.org/web/packages/tseries/) - Time series analysis and computational finance
* [quantmod](http://cran.r-project.org/web/packages/quantmod/) - Quantitative Financial Modeling Framework
* [doBy](http://cran.r-project.org/web/packages/doBy/) -  Groupwise summary statistics, general linear contrasts, population means (least-squares-means), and other utilities
* [xlsx](http://cran.r-project.org/web/packages/xlsx/) - Read, write, format Excel 2007 and Excel 97/2000/XP/2003 files
* [shiny](http://cran.r-project.org/web/packages/shiny/) - Web Application Framework for R
* [RCurl](http://cran.r-project.org/web/packages/RCurl/index.html) - General network (HTTP/FTP/...) client interface for R

```bash To install rgdal package
apt-get install libcurl-dev
```

* [devtools](http://cran.r-project.org/web/packages/devtools/) - Tools to make developing R code easier
* [gplots](http://cran.r-project.org/web/packages/gplots/) - Various R programming tools for plotting data
* [ggmap](http://cran.r-project.org/web/packages/ggmap/) - A package for spatial visualization with Google Maps and OpenStreetMap
* [googleVis](http://cran.r-project.org/web/packages/googleVis/) - Interface between R and the Google Chart Tools
* [rworldmap](http://cran.r-project.org/web/packages/rworldmap/) - Mapping global data, vector and raster.
* [rgdal](http://cran.r-project.org/web/packages/rgdal/) - Bindings for the Geospatial Data Abstraction Library

```bash To install rgdal package
apt-get install libgdal1-dev
apt-get install libproj-dev # or proj if not found
```

* [ROracle](http://cran.r-project.org/web/packages/ROracle/) - Oracle database interface (DBI) driver for R.

    ROracle need Oracle Client install, and further additional [setup procedure](/2012/09/16/install-r-and-rstudio-in-ubuntu/)

