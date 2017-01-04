# spark_cluster_vagrant #
Vagrant template to install a standalone Spark cluster with lean defaults.

# Details #

- See `Vagrantfile` for details and to make changes.
- Spark 2.1.0 running as a standalone cluster.
- One headnode Ubuntu 16.04 machine and `N` worker (slave) machines.
- `avahi-daemon` for local DNS resolution.

# TODO #

- Start master and slave services on boot, not at end of provisioning.

# Testing #

To run `SparkPi` on the cluster, run the following command from the headnode `hn0`:

    spark-submit --master spark://hn0.local:7077 --class org.apache.spark.examples.SparkPi ~/spark-2.1.0-bin-hadoop2.7/examples/jars/spark-examples_2.11-2.1.0.jar 1000
