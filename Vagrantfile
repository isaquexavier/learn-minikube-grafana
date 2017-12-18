Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 2
    v.customize ["modifyvm", :id, "--nictype1", "virtio"]
  end
  config.vm.box = "ubuntu/xenial64"
  # Check if they do a copy should be avoided
  config.ssh.forward_agent = true
  config.vm.define "minikube-prometheus"
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get -y update
    sudo apt-get upgrade -y
    sudo apt-get install -y git vim curl wget
    wget --progress=bar:force -O minikube https://storage.googleapis.com/minikube/releases/v0.24.1/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
  SHELL
end
