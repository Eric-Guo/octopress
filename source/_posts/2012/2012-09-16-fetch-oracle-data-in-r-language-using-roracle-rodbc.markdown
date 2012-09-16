---
layout: post
title: "Fetch Oracle DATA in R language using ROracle/RODBC and make a Bubble Charts"
date: 2012-09-16 16:23
comments: true
external-url:
categories: [R]
---
Inspired by [Nathan Yau's tutorials - How to Make Bubble Charts](http://flowingdata.com/2010/11/23/how-to-make-bubble-charts/), fetch the data directly from Oracle. <!--more-->

Using ROracle to link, notice [ROracle can only be installed on Linux](/2012/09/16/install-r-and-rstudio-in-ubuntu/), so if you are in Windows, have to use RODBC instead of ROracle.

```r PN_Circles.R
library(ROracle)
drv <- dbDriver("Oracle")
con <- dbConnect(drv, username="user", password="passwd", dbname="tnsname")
res <- dbSendQuery(con, "
SELECT pc.packagecategoryname Package,
	   replace(replace(p.technology,'nm',''),'NM','') Tech,
	   replace(p.densitycode, 'G','') Density, count(p.qaddevice) PN_Number
 FROM product p
INNER JOIN a_packagecategory pc ON pc.packagecategoryid=p.packagecategoryid
INNER JOIN a_productlevel ptl ON ptl.productlevelid=p.productlevelid
WHERE p.isavailable=1
  AND ptl.productlevelname IN ('54-82')
  AND p.densitycode NOT IN ('512M')
  AND p.technology NOT IN ('-')
  AND pc.packagecategoryname like 'INAND_'
GROUP BY pc.packagecategoryname,
	  replace(replace(p.technology,'nm',''),'NM',''),
	  replace(p.densitycode, 'G','')
")
data <- fetch(res, n=-1)
radius <- sqrt(data$PN_NUMBER/ pi)
symbols(data$DENSITY, data$TECH, circles=radius, inches=0.35,
		main = "Number of PN per nano technology and density",
		fg="white", bg="red", xlab="Density", ylab="Technology")
text(x=data$DENSITY, y=data$TECH, labels=data$PACKAGECATEGORY, cex=0.5)
```

```r using below if you are in Windows
library("RODBC")
db <- odbcConnect(dsn="mesprd", uid="mesprd", pwd="wip24ux")
data <- sqlQuery(db, "SELECT SYSDATE from DUAL")
```