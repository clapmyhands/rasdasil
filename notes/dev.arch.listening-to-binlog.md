---
id: aysqmsuilhf1t5iuu3ehw9v
title: Listening to Binlog
desc: ''
updated: 1714895188515
created: 1714895188515
tags: event-stream database compsci
---

It's possible to listen to database binlog as an event stream.
The binlog events can trigger any action based on the changed data e.g. [[Cache Invalidation]] / callback.

This might seem like overkill or wasteful if only a small subset of binlog event is of interest.
However this is an easy way of adding event handling that happens on committed record.
