---
title: "Locks, Deadlocks and Liquidbase"
date: 2021-12-30
draft: false
summary: A story of a nasty encounter with a corrupt lock caused by unexpectedly shutting down an application executing Liquidbase and the deadlocks it caused, preventing instances from booting up.
authors: Hugo Martins
categories: [ concurrency]
---

"Long time no see", heh? I've been busy with other undertakings - such as switching jobs - which means I've had less time to write. The emptiness in here since October is explained by that but I'm back and I bring some extraordinary *fun* with [Liquidbase](https://www.liquibase.org/) and its locking mechanism.

Liquidbase is an open source library designed to improve versioning of database schema changes. You can check its [source code](https://github.com/liquibase/liquibase) but, in essence, it helps controlling versioning across database schema changes, similar to [Flyway](https://flywaydb.org/), and it is written in Java, making it ideal for bootstrapping Spring applications.

Recently, I've had a nasty encounter with Liquidbase, due to its use in Spinnaker, in particular [clouddriver](https://github.com/spinnaker/clouddriver). It envolves corrupt locks and deadlocks caused by those.

Liquidbase relies on a *changelog*, composed of *changesets*, to reliable and accurately know which changes have already been executed or haven't been executed. Liquidbase uses a table in the database, named `DATABASECHANGELOG`, to track these *changesets*, whether they have already been executed and *when* they have been executed.

Because Liquidbase is aware that multiple instances of the same application might be booting up at the same time and might be reading `DATABASECHANGELOG` at the same time, or conflicting times, it also has a table named `DATABASECHANGELOGLOCK`. In this table, instances that are currently reading the database can share locks and manage themselves out of concurrency issues.

Whenever an application with Liquidbase boots up, it will read `DATABASECHANGELOGLOCK` and check for an existing lock. If the lock has been acquired by another instance, it will wait until the lock has been freed to proceed with acquiring the lock and perform the necessary operations.

Unfortunately, as we know, this behaviour is prone to situations of corrupt locks being left in the database, for instances that are no longer requiring the lock or even running, leaving all other instances locked out and unable to boot. In essence, creating a deadlock because all instances are waiting for the existing lock to be freed, while the original instance has probably terminated a long time ago.

This is exactly what happens with Liquidbase if an instance happens to be unexpectedly terminated without freeing its lock. Any other instance that tries to boot up won't be able to acquire the lock and will enter an infinite loop, until the lock is finally free.

I'm assuming this is a common occurrence because it is even documented in [Liquidbase's documentation](https://docs.liquibase.com/concepts/basic/databasechangeloglock-table.html?__hstc=94692481.33ffaa0fcf7cebaa197baf4111ff2e40.1640694727731.1640694727731.1640694727731.1&__hssc=94692481.1.1640694727731&__hsfp=1132227981&_ga=2.228451742.2022857303.1640694723-1719938394.1640694723):

> If Liquibase does not exit cleanly, the lock row may be left as locked. You can clear out the current lock by running `liquibase releaseLocks` which runs `UPDATE DATABASECHANGELOGLOCK SET LOCKED=0`

Unfortunately, I didn't know this before hand. When I encountered this issue, it made for a fun afternoon of chasing ghosts...until I saw somewhere online that there might be a corrupt lock still in the database. 

If you happen to encounter this error and see instances booting up and looping infinitely with `Waiting for changelog lock`...please make sure you aren't witnessing a deadlock due to a corrupt lock in your database.