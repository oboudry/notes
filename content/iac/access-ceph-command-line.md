+++
title = 'Access Ceph Command Line'
date = 2025-06-04T17:14:18+02:00
draft = false
tags = ["ceph", "kubernetes"]
+++

## Accessing the Ceph console on a Kubernetes cluster

Exec command `bash` into the `rook-ceph-tools` deployment.

```sh
$ kubectl -n rook-ceph exec -it deploy/rook-ceph-tools -- bash
bash-4.4$ ceph status
  cluster:
    id:     84943fa1-1b6a-4e0b-bacb-b8f4bcdb0798
    health: HEALTH_OK

  services:
    mon: 5 daemons, quorum k,l,m,o,p (age 10h)
    mgr: b(active, since 2d), standbys: a
    mds: 1/1 daemons up, 1 hot standby
    osd: 5 osds: 5 up (since 13d), 5 in (since 10w)

  data:
    volumes: 1/1 healthy
    pools:   4 pools, 177 pgs
    objects: 98.94k objects, 175 GiB
    usage:   525 GiB used, 623 GiB / 1.1 TiB avail
    pgs:     177 active+clean

  io:
    client:   894 KiB/s rd, 960 KiB/s wr, 3 op/s rd, 9 op/s wr

bash-4.4$
```
