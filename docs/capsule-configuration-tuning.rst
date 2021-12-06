Capsule Configuration Tuning
============================

Capsules (called Smart Proxies in upstream Foreman) are meant to offload part of Satellite load related to distributing content to clients but they can also be used to execute Remote Execution jobs. What they can not help with is anything which extensively uses Satellite API as host registration or package profile update.

We have measured multiple test cases on multiple Capsule 6.10 configurations:

+--------------------------+----------+------------------+
| Capsule HW configuration |   CPUs   |    memory        |
+==========================+==========+==================+
|      minimal             |    4     |      12 GB       |
+--------------------------+----------+------------------+
|      large               |    8     |      24 GB       |
+--------------------------+----------+------------------+
|      extra large         |    16    |      46 GB       |
+--------------------------+----------+------------------+


Content delivery use case
-----------------------------

In a download test where we concurrently downloaded a 40MB repo of 2000 packages on 100, 200, .. 1000 hosts, we saw roughly 50% improvement in average download duration every time when we doubled Capsule resources. See table below for more concrete numbers..

+--------------------------------+-------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
|  Concurrent downloading hosts  |    Minimal (4CPU and 12GB Mem) -> Large (8CPU and 24GB Mem)                                     |  Large (8CPU and 24GB Mem) -> Extra Large (16CPU and 46GB Mem)                                 |   Minimal (4CPU and 12GB Mem) -> Extra Large (16CPU and 46GB Mem)                            |
+--------------------------------+-------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
|  Avg Improvement               |    ~ 50% (e.g. for 700 concurrent downloads in average 9 seconds vs. 4.4 seconds per package)   |  ~ 40% (e.g. for 700 concurrent downloads in average 4.4 seconds vs. 2.5 seconds per package)  |  ~ 70% (e.g. for 700 concurrent downloads in average 9 seconds vs. 2.5 seconds per package)  |
+--------------------------------+-------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------+

When we compared download performance from Satellite vs. from Capsule we have seen only about 5% speedup, but that is expected as Capsule’s main benefit is in getting content closer to geographically distributed clients (or clients in different networks) and in handling part of the load Satellite would have to handle itself. In some smaller HW configurations (8 CPUs and 24 GB) Satellite was not able to handle downloads from more than 500 concurrent clients, while a capsule with the same HW configuration was able to service more than 1000 and possibly even more.

Frequent registrations use case
-----------------------------------

For concurrent registrations a bottleneck is CPU speed, but all configs were able to handle even high concurrency without swapping. HW resources used for Capsule have only minimal impact on registration performance. E.g. for capsule with 16 CPUs and 46 GB of RAM we have seen at most 9% of registration speed improvement when compared to a capsule with 4 CPUs and 12 GB RAM.

Remote execution use case
-----------------------------

We have tested executing Remote Execution jobs via both SSH and Ansible backend on 500, 2000 and 4000 hosts. All configurations were able to handle all of the tests without errors, except for the smallest configuration (4CPUs and 12 GB memory) which failed to finish on all 4000 hosts.

In a sync test where we synced RHEL 6, 7, 8 BaseOS and 8 AppStream we have not seen significant differences amongst Capsule configurations. This will be different for syncing a higher number of content views in parallel.



