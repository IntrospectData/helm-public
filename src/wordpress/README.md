# Wordpress, Percona and Shared Storage

What is this chart? It's a scalable take on deploying [Wordpress](https://hub.docker.com/_/wordpress/) with the [Percona XtraDB Cluster](https://www.percona.com/software/mysql-database/percona-xtradb-cluster) toolset and, optionally, some good 'ol fashioned [NFS](https://github.com/helm/charts/tree/master/stable/nfs-server-provisioner) for shared storage.

The `values.yaml` file here is fairly straightforward, but there are a few things to be aware of:

* if you enable nfs storage and you enable the creation of a `storageClass`, the `storageClass` for the PVC of the deployment will use the `storageClass` name from the nfs-server-provisioner portion of the values, not the one specified more globally.
* We're lazy when it comes to configuration values... for the most part, we use the `envFrom` directive and simply use a `configMap` and `secret` to hold environment variables.
  * wordpress salt/hashes go in the `secret`
  * most other wordpress env variables documented for the [container] go into the `configMap`
  * we specifically reference the database password via the secret created by the percona db chart. Otherwise, it goes into the secret as well.
