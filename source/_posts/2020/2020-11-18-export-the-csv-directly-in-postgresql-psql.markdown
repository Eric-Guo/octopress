---
layout: post
title: "Export the CSV directly in postgresql psql"
date: 2020-11-18T13:10:43+08:00
comments: true
external-url: 
categories: [CSV, PostgreSQL]
---

## Export the data from SQL

```bash
psql -d postgres
\copy (SELECT vote_options.title, vote_options.vote_index, vote_options.user_votes_count, (DENSE_RANK() OVER(ORDER BY user_votes_count DESC ) ) as rank FROM "vote_options" WHERE "vote_options"."vote_id" = 1 ORDER BY "rank") to 'vote_rank.csv' with csv
```
