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

* [pls](https://cran.r-project.org/web/packages/pls/) - Multivariate regression methods Partial Least Squares Regression (PLSR), Principal Component Regression (PCR) and Canonical Powered Partial Least Squares (CPPLS).
* [assertthat](https://cran.r-project.org/web/packages/assertthat/) - assertthat is an extension to stopifnot() that makes it easy to declare the pre and post conditions that you code should satisfy.
* [dplyr](https://cran.r-project.org/web/packages/dplyr/) - A fast, consistent tool for working with data frame like objects, both in memory and out of memory.
* [data.table](https://cran.r-project.org/web/packages/data.table/) - Fast aggregation of large data (e.g. 100GB in RAM), fast ordered joins, fast add/modify/delete of columns by group using no copies at all, list columns and a fast file reader (fread). Offers a natural and flexible syntax, for faster development.
* [earth](https://cran.r-project.org/web/packages/earth/) - Build regression models using the techniques in Friedman's papers "Fast MARS" and "Multivariate Adaptive Regression Splines". 
* [kernlab](https://cran.r-project.org/web/packages/kernlab/) - Kernel-based machine learning methods for classification, regression, clustering, novelty detection, quantile regression and dimensionality reduction. Among other methods kernlab includes Support Vector Machines, Spectral Clustering, Kernel PCA, Gaussian Processes and a QP solver.
* [caret](https://cran.r-project.org/web/packages/caret/) - Classification and Regression Training, Misc functions for training and plotting classification and regression models.
* [rpart](https://cran.r-project.org/web/packages/rpart/) - Recursive Partitioning and Regression Trees
* [party](https://cran.r-project.org/web/packages/party/) - A Laboratory for Recursive Partytioning
* [rJava](https://cran.r-project.org/web/packages/rJava/) - Low-Level R to Java Interface to support RWeka
* [RWeka](https://cran.r-project.org/web/packages/RWeka/) - An R interface to Weka (Version 3.7.12). Weka is a collection of machine learning algorithms for data mining tasks written in Java, containing tools for data pre-processing, classification, regression, clustering, association rules, and visualization.
* [ipred](https://cran.r-project.org/web/packages/ipred/) - Improved predictive models by indirect classification and bagging for classification, regression and survival problems as well as resampling based estimators of prediction error.
* [randomForest](https://cran.r-project.org/web/packages/randomForest/) - Breiman and Cutler's random forests for classification and regression
* [gbm](https://cran.r-project.org/web/packages/gbm/) - Generalized Boosted Regression Models
* [Cubist](https://cran.r-project.org/web/packages/Cubist/) - Regression modeling using rules with added instance-based corrections
* [VGAM](https://cran.r-project.org/web/packages/VGAM/) - Vector Generalized Linear and Additive Models, An implementation of about 6 major classes of statistical regression models. At the heart of it are the vector generalized linear and additive model (VGLM/VGAM) classes. Currently only fixed-effects models are implemented, i.e., no random-effects models. Many (150+) models and distributions are estimated by maximum likelihood estimation (MLE) or penalized MLE, using Fisher scoring.
* [mda](https://cran.r-project.org/web/packages/mda/) - Mixture and flexible discriminant analysis, multivariate adaptive regression splines (MARS), BRUTO...
* [klaR](https://cran.r-project.org/web/packages/klaR/) - Miscellaneous functions for classification and visualization developed at the Fakultaet Statistik, Technische Universitaet Dortmund
* [e1071](https://cran.r-project.org/web/packages/e1071/) - Misc Functions of the Department of Statistics, Probability Theory Group (Formerly: E1071), TU Wien
* [C50](https://cran.r-project.org/web/packages/C50/) - C5.0 decision trees and rule-based models for pattern recognition.
* [ROCR](https://cran.r-project.org/web/packages/ROCR/) - Visualizing the Performance of Scoring Classifiers
* [ISLR](https://cran.r-project.org/web/packages/ISLR/) - Data for An Introduction to Statistical Learning with Applications in R

