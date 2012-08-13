---
layout: post
title: "A true story about upgrade Crystal Report from Oracle 9i to Oracle 11gR2"
date: 2011-12-18 15:16
comments: true
external-url:
categories: [Oracle, Crystal Report]
---
Few months ago, my company decides and proceeds to upgrade our factory <a href="http://en.wikipedia.org/wiki/Manufacturing_execution_system" target="_blank">MES</a> system, we create a new database for the new MES system, using Oracle 11gR2 instead of previous Oracle 9i, and we also create a new user and its schema <em>meswip45</em> instead of previous <em>mesprd on the new 11g database</em>.

Every program which link to the MES system was upgrade side by side, one after another according to the project plan, it is barely smoothly according to the plan. Now, we will make MES system go live to the production in 2012/1/2 and it's the best time point as it is most idle time in a year and just following by Christmas and Chinese Spring Festival.

But during last two weeks <a href="http://en.wikipedia.org/wiki/Acceptance_testing#User_acceptance_testing" target="_blank">UAT</a>, one of our user found the Travel Card (just the Crystal Reporting business alternative name, you can read it as a specially report) cannot output via printer, <!--more-->after check the one of the slowest Crystal Reporting, we can easily conclude that there is no program logic bug to block the Crystal Travel Card printing. It is just the reporting execution time is unreasonable long (more than 2 minutes to output the actual content).

The first came out guess is there must be some index missing, but after checking Performance Information (in the report menu) in Crystal Report, there is no SQL slow, the main report and all sub report are both running less than 50ms. Since the phenomenon is only appears in new database and after I try to set the reporting new Datasource Location (in the database menu), I have to suspect there must be something wrong in Crystal Report when switch the Datasource.

When database schema changed, the Crystal Reporting will auto refresh each of it used tables and trying to mapping the new change automatically. We known and do the refresh procedure every time we change the database table structure in the past when it is needed to add new field/column to the legacy MES. So it still reasonable that Crystal Report takes a very long time at first time since it must read and probe all the database table change in new MES 4.5 schema. But after successfully fresh and update 4.5 database schema changes and save the Crystal Report files. Reopen and rerun the report should be very fast, but I still found the new reporting file performance is still as bad as the first time!

Definitely it is a bug! But after assure that I already use the most patched Crystal Report (<a href="https://websmp230.sap-ag.de/sap(bD16aCZjPTAwMQ==)/bc/bsp/spn/bobj_download/main.htm" target="_blank">Service Pack 6</a>, version 11.5.12.1838), we have almost no option to improve: the Crystal Report XI is release at 2005 and in EOL(End-Of-Live) period, I don't think upgrade to 2008/2011 for the Crystal Report will certainly solve the problem, plus only two weeks time left and the procurement cycle/purchase policy also cannot fulfill the requirement. Crystal Report is also not an open source product either, so even I have the enough knowledge/willing to check its source code, there is still no access. (Maybe such is a good example why open source is a better solution for users)

So I try to change the solving problem direction, since change the Crystal Report Datasource introduce the above bug, why not change the Oracle database user and schema same as previous? I create the previous user and setting up the Synonym link to the new user schema using below SQL report SQL:

```sql Create Synonym SQLs
SELECT 'CREATE OR REPLACE SYNONYM '||u.TABLE_NAME||' for '||u.OWNER||'.'||u.TABLE_NAME||' ;'
FROM all_tables u
WHERE u.OWNER='MESWIP45'
```

After that, there is no need to switch Datasource in Crystal Reporting now, what I need to do is only change the ODBC link use in before Crystal Report and point to the new database.

It's cool and thanks the Oracle synonym feature! After try to running and create some missing other database link table views problem, the Crystal Reporting running in the legacy <em>mesprd </em>schema in the new Oracle 11gR2 database! I refresh the Crystal Report, save the schema changed and done! &hellip;But wait, even this time the database switch is avoid, but seems the <strong>second</strong> reopen printing of the Travel Card performance is still bad.

I return to the starting point again with a conclusion: Switching/setting new Datasource in Crystal Report have <strong>no</strong> effect.&nbsp; There might be no bug in crystal report Set Datasource feature but <strong>do</strong> have a bug existing after migrate new database from 9i to 11gR2 in ODBC link.

After read the conclusion several times, suddenly I found the word ODBC, <a href="http://en.wikipedia.org/wiki/ODBC" target="_blank">Open DataBase Connectivity</a> is MS invented database technology, if it cannot work, why not change another way to link to Oracle?

{% img /images/2011/crystal_report_support_data_sources.png Crystal Report supported data sources %}

I found the Oracle Server can be a good candidate to replace ODBC link and after fill the TNSNames, User ID and Password, a new Datasource to replace the legacy ODBC Datasource is ready to try again. I tried and this time, the bug is finally solved.