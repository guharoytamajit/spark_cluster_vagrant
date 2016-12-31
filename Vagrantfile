Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network", type: "dhcp"

  # head node:
  config.vm.define "hn0" do |node|
    node.vm.hostname = "hn0"

    node.vm.provider "virtualbox" do |vb|
      vb.name = "hn0"
      vb.gui = false
      vb.memory = 2048
      vb.cpus = 2
    end

    node.vm.provision "shell" do |s|
      s.inline = "apt-get install -y python"
    end

    node.vm.provision "ansible" do |ansible|
      ansible.playbook = "hn.yml"
      ansible.sudo = true
    end

    node.vm.provision "shell", inline: <<-SHELL
      echo SPARK_LOCAL_IP=hn0.local >> /home/ubuntu/spark-2.1.0-bin-hadoop2.7/conf/spark-env.sh
      echo SPARK_MASTER_IP=hn0.local >> /home/ubuntu/spark-2.1.0-bin-hadoop2.7/conf/spark-env.sh
      /home/ubuntu/spark-2.1.0-bin-hadoop2.7/sbin/start-master.sh -h hn0.local
    SHELL
  end

  # worker nodes:
  (0..0).each do |i|
    config.vm.define "wn#{i}" do |node|
      node.vm.hostname = "wn#{i}"

      node.vm.provider "virtualbox" do |vb|
        vb.name = "wn#{i}"
        vb.gui = false
        vb.memory = 4096
        vb.cpus = 2
      end

      node.vm.provision "shell" do |s|
        s.inline = "apt-get install -y python"
      end

      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "wn.yml"
        ansible.sudo = true
      end

      node.vm.provision "shell", inline: <<-SHELL
        echo SPARK_LOCAL_IP=wn#{i}.local >> /home/ubuntu/spark-2.1.0-bin-hadoop2.7/conf/spark-env.sh
        echo SPARK_MASTER_IP=hn0.local >> /home/ubuntu/spark-2.1.0-bin-hadoop2.7/conf/spark-env.sh
        /home/ubuntu/spark-2.1.0-bin-hadoop2.7/sbin/start-slave.sh -h wn#{i}.local spark://hn0.local:7077
      SHELL
    end
  end
end
