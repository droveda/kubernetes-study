Vagrant.configure("2") do |config|

    config.vm.box = "ubuntu/bionic64"
  
    config.vm.define "kubernetes_docker_study" do |kubernetes_docker_study|
        kubernetes_docker_study.vm.network "public_network", ip: "192.168.100.131"
  
        kubernetes_docker_study.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
        vb.name = "ubuntu_kubernetes_docker_study"
        vb.customize ['modifyvm', :id, '--nested-hw-virt', 'on']
      end
      kubernetes_docker_study.vm.provision "shell", inline: "apt-get update && apt-get install -y docker.io"
    end
    
  end