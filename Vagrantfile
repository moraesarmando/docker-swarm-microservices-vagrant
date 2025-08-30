Vagrant.configure("2") do |config|
  # Manager node
  config.vm.define "manager" do |manager|
    manager.vm.box = "ubuntu/bionic64"
    manager.vm.hostname = "manager"
    manager.vm.network "private_network", type: "dhcp"
    manager.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 2
    end
    manager.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y docker.io
      systemctl start docker
      systemctl enable docker
      docker swarm init --advertise-addr $(hostname -I | awk '{print $2}')
      docker swarm join-token worker -q > /vagrant/worker_token
    SHELL
    manager.vm.synced_folder ".", "/vagrant"
  end

  # Worker nodes
  (1..2).each do |i|
    config.vm.define "worker#{i}" do |worker|
      worker.vm.box = "ubuntu/bionic64"
      worker.vm.hostname = "worker#{i}"
      worker.vm.network "private_network", type: "dhcp"
      worker.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 2
      end
      worker.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y docker.io
        systemctl start docker
        systemctl enable docker
        while [ ! -f /vagrant/worker_token ]; do sleep 2; done
        MANAGER_IP=$(getent hosts manager | awk '{ print $1 }')
        TOKEN=$(cat /vagrant/worker_token)
        docker swarm join --token $TOKEN $MANAGER_IP:2377
      SHELL
      worker.vm.synced_folder ".", "/vagrant"
    end
  end
end