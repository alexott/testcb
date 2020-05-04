# Collection of diagnostic data

There are several ways to collect necessary diagnostic data (in order of preference):
* Use diagnostic scripts from https://github.com/DataStax-Toolkit/diagnostic-collection - they work with both DSE, DDAC & Cassandra. Scripts collects both Cassandra/DSE-related information, together with system information. Just grab `diagnostic-scripts-<VERSION>.tar.gz` file from https://github.com/DataStax-Toolkit/diagnostic-collection/releases and use them as described in `README.md` 
* (DSE-only) Use OpsCenter to generate diagnostic tarball. Although for customer it’s much easier to use UI, the generation of tarball often fails on big clusters, and it doesn’t include much system information
* Collect system information "manually" (although it would be easier to use `collect_node_diag.sh` from diagnostic-collection on every node & send us generated files, as this method is potentially error-prone and doesn’t allow to use existing diagnostic tools easily:
  * On one node of the cluster execute:
    ```sh
    cqlsh -e "describe full schema" > schema.full
    ```
  * on every node of cluster and place the outputs/files in a directory with the node's IP address as the directory name:
    * Execute following commands:
      ```sh
      nodetool status > status
      nodetool info > info
      nodetool cfstats > cfstats
      nodetool cfhistograms > cfhistograms
      nodetool compactionstats > compactionstats
      nodetool describecluster > describecluster
      nodetool gossipinfo > gossipinfo
      nodetool proxyhistograms > proxyhistograms
      nodetool netstats > netstats
      nodetool tpstats > tpstats
      java -version > java-version.txt
      free -m > free-m.txt
      df -h > df-h.txt
      ```
    * Copy log files:
      * `system.log` (usually at `/var/log/cassandra`)
      * `debug.log`  (usually at `/var/log/cassandra`)
      * `gc.log` (usually at `/var/log/cassandra`)
      * `output.log` (usually at `/var/log/cassandra`)
    * Configuration files:
      * `cassandra.yaml` (usually at `/etc/cassandra/` for Cassandra, or `/etc/dse/cassandra/` for DSE)
      * `cassandra-env.sh` (usually at `/etc/cassandra/` for Cassandra, or `/etc/dse/cassandra/` for DSE)
      * `jvm*.options` (usually at `/etc/cassandra/` for Cassandra, or `/etc/dse/cassandra/` for DSE)
      * `dse.yaml` (DSE-only, usually in the `/etc/dse/`)
  * Collected data should be packed as a single file and delivered to DataStax.

