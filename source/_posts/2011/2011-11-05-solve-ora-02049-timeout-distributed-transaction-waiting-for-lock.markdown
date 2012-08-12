---
layout: post
title: "Solve ORA-02049: timeout: distributed transaction waiting for lock"
date: 2011-11-05 15:06
comments: true
external-url:
categories: [Oracle, Camstar]
---
In Oct 28 2011, while I was working to recover a production reporting oracle database sync SQL facility provide by MES vendor <a href="http://www.camstar.com/" target="_blank">Camstar</a>, I meet the <a href="http://www.dba-oracle.com/sf_ora_02049_timeout_distributed_transaction_waiting_for_lock.htm" target="_blank">ORA-02049</a> error, Such error is never observed, but after <a href="http://www.oracle-base.com/articles/misc/KillingOracleSessions.php" target="_blank">kill session</a> relative with sync SQL in production transaction database, the whole sync SQL facility return to normal.

So except the reason suggested by the <em>Burleson Consulting</em>, the root cause in my case can be the reporting server suddenly down and in it&rsquo;s <a href="http://www.dba-oracle.com/t_two_phase_commit_2pc.htm" target="_blank">2 phase commit</a>, there is exist a lock in transaction database but rebooted reporting server&rsquo;s oracle now can not known it. so after kill that pending session, the ORA-02049 error disappeared.

Suddenly UNIX server reboot/memory dump is still possible, so I record here and hoping its useful for you.