# ansible_spark_lean
Vagrant template to install a Spark cluster with lean defaults.

# Details #

- See `Vagrantfile` for details and to make changes.
- Spark 2.1.0 running as a standalone cluster.
- One headnode Ubuntu 16.04 machine and `N` worker (slave) machines.
- Uses Ansible for provisioning. Might change in the future.

# TODO #

- Start slave on head node?
- Start master and slave services on boot, not at end of provisioning.
- Get rid of Ansible? It's a pain more than anything for something this small.
- Don't use avahi-daemon for hostnames/IPs, not everyone has it.

# Testing #

To run `SparkPi` on the cluster, run the following command from the headnode `hn0`:

    spark-submit --master spark://hn0.local:7077 --class org.apache.spark.examples.SparkPi ~/spark-2.1.0-bin-hadoop2.7/examples/jars/spark-examples_2.11-2.1.0.jar 1000
