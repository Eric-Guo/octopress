---
layout: post
title: "Unlock the locked table by kill session"
date: 2014-06-09T15:27:49+08:00
comments: true
external-url:
categories: [Oracle]
---

Seems I frequently need to kill some session due to the network issue or other client issue to submit data to oracle, but each time I google the answner, it takes me at least 5 minutes, so I decide to write done SQL and command here:

```sql Find the locked session
SELECT a.sid,a.serial#, a.username,c.os_user_name,a.terminal,
b.object_id,substr(b.object_name,1,40) object_name
from v$session a, dba_objects b, v$locked_object c
where a.sid = c.session_id
and b.object_id = c.object_id
```

```sql Kill session
alter system kill session '14,397';
```