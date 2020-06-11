---
layout: post
title: "Bug or correct behavior but very miss leading LEFT JOIN in MySQL 8"
date: 2020-06-11T11:35:50+08:00
comments: true
external-url:
categories:
---

Today I found a [missing rows in MySQL 8](https://stackoverflow.com/questions/33555581/mysql-left-join-function-results-missing-rows) problem in production, which SQL LEFT JOIN should be added at join condition instead of where.

Missing data version:

```sql
SELECT SUM(markettotal) markettotal
FROM `SUB_COMPANY_REAL_RECEIVE`
LEFT JOIN ORG_REPORT_DEPT_ORDER on ORG_REPORT_DEPT_ORDER.编号 = SUB_COMPANY_REAL_RECEIVE.deptcode_sum
WHERE `SUB_COMPANY_REAL_RECEIVE`.`realdate` BETWEEN '2020-01-01' AND '2020-06-30'
AND (ORG_REPORT_DEPT_ORDER.开始时间 <= '2020-06-11')
AND (ORG_REPORT_DEPT_ORDER.结束时间 IS NULL OR ORG_REPORT_DEPT_ORDER.结束时间 >= '2020-06-11')
AND `SUB_COMPANY_REAL_RECEIVE`.`orgcode_sum` IN ('H000109', '000109')
```

Correct version:

```sql
SELECT sum(markettotal)
FROM `SUB_COMPANY_REAL_RECEIVE`
LEFT JOIN ORG_REPORT_DEPT_ORDER ON SUB_COMPANY_REAL_RECEIVE.deptcode = ORG_REPORT_DEPT_ORDER.编号
AND ((ORG_REPORT_DEPT_ORDER.开始时间 <= '2020-06-11') AND (ORG_REPORT_DEPT_ORDER.结束时间 IS NULL OR ORG_REPORT_DEPT_ORDER.结束时间 >= '2020-06-11'))
WHERE `SUB_COMPANY_REAL_RECEIVE`.`realdate` BETWEEN '2020-01-01' AND '2020-06-30'
  AND `SUB_COMPANY_REAL_RECEIVE`.`orgcode_sum` IN ('H000109')
```
