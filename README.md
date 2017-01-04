# spark_cluster_vagrant #
Vagrant template to install a standalone Spark cluster with lean defaults.

# Details #

- See `Vagrantfile` for details and to make changes.
- Spark 2.1.0 running as a standalone cluster.
- One head node Ubuntu 16.04 machine and `N` worker (slave) machines.
- `avahi-daemon` for local DNS resolution.

# Usage #

To spin up your own local Spark cluster, clone this repository and open up `Vagrantfile` in a text editor.

You'll want to change the `N_WORKERS` variable near the top of the file.

Vagrant will spin up one "head node" and `N` worker nodes in a Spark standalone cluster.

Feel free to make other changes, e.g. RAM and CPU counts for each of the machines.

When you're ready, just run `vagrant up` in the directory the `Vagrantfile` is in. Wait a few minutes and your Spark cluster will be ready. SSH in using `vagrant ssh hn0` or `vagrant ssh wn4`. You'll also be able to see the Spark WebUI at `http://hn0.local:8080`.

Shut down the cluster with `vagrant halt` and delete it with `vagrant destroy`. You can always run `vagrant up` to turn on or build a brand new cluster.

# Testing #

To run `SparkPi` on the cluster, run the following command from the headnode `hn0`:

    spark-submit --master spark://hn0.local:7077 --class org.apache.spark.examples.SparkPi ~/spark-2.1.0-bin-hadoop2.7/examples/jars/spark-examples_2.11-2.1.0.jar 1000
