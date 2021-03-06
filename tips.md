---
layout: layout
title: Tips
subtitle: miscellaneous features
---
# Tips

## Query/Target persistence

Use `--stats` and related options of `norikra start`.

* `--stats=PATH`
  * JSON document, stores:
    * configurations: listen address/port, threads, logging
    * targets and queries
  * loaded on process bootstrapping (missing: ignored)
  * stored on process shutdown (overwrite current file)
* `--suppress-dump-stat`
  * specify not to store stats on process shutdown
* `--dump-stat-interval=N`
  * store stats dump per specified N seconds (ex: 1800)

Stats will be dumped with SIGUSR2 if `--stats=PATH` specified. Or, in client side, `norikra-client admin stats` can export stats json data.

## Query grouping

To fetch query output, we have 3 options:

* `events` RPC (`norikra-client event fetch`)
  * to fetch output events from a query, and remove these events from server
* `see` RPC (`norikra-client event see`)
  * to fetch output events from a query, without removing these events
* `sweep` RPC (`norikra-client event sweep`)
  * to fetch output events from queries with same query group name, and remove these events from server

We can fetch many output events from many related queries (per service, per customers, ...) from norikra server by `sweep`. Query group name is used for this purpose.

## Loopback query group

Norikra connects query outputs into other target input by specifying query group as `LOOPBACK(target_name)`.

Once you regisitered a query with loopback query group, norikra does:
 1. check `target_name` format
 1. open `target_name` if it is not opened yet
 1. register the query
 1. define fields of `target_name` by outputs of the query

Loopback query group is supported on Norikra v1.0.0 or later.
