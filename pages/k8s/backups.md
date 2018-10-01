---
title: "Backups | Canonical Distribution of Kubernetes&reg;"
keywords: juju, backup, etcd
tags: [operating]
sidebar: k8smain-sidebar
permalink: backups.html
layout: base
toc: False
summary: This page is currently a work in progress. For existing documentation, please visit <a href="https://kubernetes.io/docs/getting-started-guides/ubuntu/"> https://kubernetes.io/docs/getting-started-guides/ubuntu/ </a>
---

# Backups

The state of your Kubernetes cluster is kept in the etcd datastore. This page
shows how to backup and restore the etcd included in the **Canonical
Distribution of Kubernetes <sup>&reg;</sup>**.

 Backing up application specific data, normally stored in a persistent volume, is not
 currently supported by native Kubernetes. Various third party solutions are available -
 please refer to their own documentation for details.

 ## Creating an **etcd** snapshot

 **etcd** is a distributed key/value store. To create a snapshot, all that is required is to run
 the `snapshot` action on one of the units running **etcd**:

 ```bash
 juju run-action etcd/0 snapshot --wait
 ```

 By specifying `--wait`, the console will wait to return the result of running
 the action, which in this case includes the path and filename of the generated
 snapshot file. For example:

 ```bash
 unit-etcd-0:
  id: 3d6a505e-07d7-4697-8471-60156f87b1b4
  results:
    copy:
      cmd: juju scp etcd/0:/home/ubuntu/etcd-snapshots/etcd-snapshot-2018-09-26-18.04.02.tar.gz
        .
    snapshot:
      path: /home/ubuntu/etcd-snapshots/etcd-snapshot-2018-09-26-18.04.02.tar.gz
      sha256: e85ae4d49b6a889de2063031379ab320cc8f09b6e328cdff2fb9179fc641eee9
      size: 68K
      version: |-
        etcdctl version: 3.2.10
        API version: 2
  status: completed
  timing:
    completed: 2018-09-26 18:04:04 +0000 UTC
    enqueued: 2018-09-26 18:04:04 +0000 UTC
    started: 2018-09-26 18:04:03 +0000 UTC
  unit: etcd/0
  ```

  This path/filename relates to the unit where the action was run. As we will
  likely want to use the snapshot on a different unit, we should fetch the
  snapshot to the local machine. The command to perform this is also helpfully
  supplied in the `copy` section of the output (see above):

```bash
  juju scp etcd/0:/home/ubuntu/etcd-snapshots/etcd-snapshot-2018-09-26-18.04.02.tar.gz .
  ```

It is also wise to check the sha256 checksum of the file you have copied
against the value in the previous output:

```bash
sha256sum etcd-snapshot-2018-09-26-18.04.02.tar.gz
```

## Restoring a snapshot

<div class="p-notification--warning"><p class="p-notification__response">
<span class="p-notification__status">Warning:</span>
Restoring a snapshot should not be performed when there is more than one unit of **etcd** running.
 </p></div>
