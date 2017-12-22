Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.gui = true
    v.memory = 4096
    v.cpus = 2
    v.customize ["modifyvm", :id, "--nictype1", "virtio"]
    v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    v.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
  end
  config.vm.box = "ubuntu/xenial64"
  # Check if they do a copy should be avoided
  config.ssh.forward_agent = true
  config.vm.network "forwarded_port", guest: 30000, host: 30000, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 30002, host: 30002, host_ip: "127.0.0.1"
  config.vm.define "minikube-prometheus"

  # Install xfce and virtualbox additions
  config.vm.provision "shell", inline: "sudo apt-get update"
  config.vm.provision "shell", inline: "sudo apt-get install -y xfce4 virtualbox-guest-dkms virtualbox-guest-utils virtualbox-guest-x11"
  config.vm.provision "shell", inline: "sudo apt-get install xserver-xorg-legacy"
  # Permit anyone to start the GUI
  config.vm.provision "shell", inline: "sudo sed -i 's/allowed_users=.*$/allowed_users=anybody/' /etc/X11/Xwrapper.config"

  config.vm.provision "shell", inline: <<-SHELL

    # preparation
    sudo apt-get -y update
    sudo apt-get upgrade -y
    sudo apt-get install -y git vim curl wget

    # Chrome
    wget --progress=bar:force -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
    echo 'deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main' | sudo tee /etc/apt/sources.list.d/google-chrome.list
    sudo apt-get update
    sudo apt-get install -y google-chrome-stable

    # docker
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt-get update
    apt-cache policy docker-ce
    sudo apt-get install -y docker-ce
    sudo systemctl status docker
    sudo usermod -aG docker ubuntu

    # creating user for Desktop login
    # sudo useradd -m -p userone userone
    # sudo usermod -aG docker userone
    # sudo usermod -aG sudo userone

    # minikube + kubectl
    wget --progress=bar:force -O kubectl https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
    wget --progress=bar:force -O minikube https://storage.googleapis.com/minikube/releases/v0.24.1/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
    sudo export CHANGE_MINIKUBE_NONE_USER=true
    sudo minikube start --vm-driver=none &&

    # enabling addons
    sudo minikube addons enable heapster &&

    # changing permissions
    sudo minikube stop &&
    sudo minikube start --vm-driver=none &&
    sudo mv /root/.kube /home/ubuntu/.kube &&
    sudo chown -R ubuntu /home/ubuntu/.kube &&
    sudo chgrp -R ubuntu /home/ubuntu/.kube &&
    sudo mv /root/.minikube /home/ubuntu/.minikube &&
    sudo chown -R ubuntu /home/ubuntu/.minikube &&
    sudo chgrp -R ubuntu /home/ubuntu/.minikube

  SHELL
end
