Vagrant.configure("2") do |config|
    # Master node
    config.vm.define "master" do |master|
      master.vm.box = "generic/ubuntu2004"
      master.vm.hostname = "master"
      master.vm.network "private_network", type: "dhcp"
      master.vm.provision "shell", path: "kube-config.sh"
    end
  
    # Worker nodes
    (1..2).each do |i|
      config.vm.define "worker#{i}" do |worker|
        worker.vm.box = "generic/ubuntu2004"
        worker.vm.hostname = "worker#{i}"
        worker.vm.network "private_network", type: "dhcp"
        worker.vm.provision "shell", path: "kube-config.sh"
      end
    end
  end