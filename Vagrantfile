## Variables
$name = "DevOps"
$host_ip = "192.168.56.100"
$port= 8501
$vm_memory = 1024
$devops_dir   = "/home/vagrant/devops"
#Vagrant configuration
Vagrant.configure("2") do |config|
  config.vm.box = "chenhan/ubuntu-desktop-20.04"
  config.vm.hostname = $name
  config.vm.provider "virtualbox" do |v|
     v.memory = $vm_memory
     v.cpus   = 2
     v.name   = $name
     v.gui    = false
     v.customize ["modifyvm", :id, "--monitorcount", "1"]
     v.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
     v.customize ["modifyvm", :id, "--vram", "128"]
     v.customize ["modifyvm", :id, "--name", "Devops"]
     v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    
  end
  config.vm.provision "shell", inline: <<-SHELL
    #!/bin/bash
    sudo apt-get update
    sudo apt install apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt-get update
    sudo apt-get install -y docker-ce 
    sudo usermod -aG docker vagrant
    sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
  SHELL

  config.vm.network "forwarded_port", guest: $port, host: $port
  config.vm.network :private_network, ip: $host_ip
  config.vm.synced_folder ".", "/home/vagrant/devops", create: true, owner: "vagrant"

  config.vm.provision :docker
  
  config.vm.provision :docker_compose,
    #executable: "/home/vagrant/devops/docker-compose.yml", ## Specifying 'executable' option to avoid CoreOS restriction
    yml: "/home/vagrant/devops/docker-compose.yml",
    run: "always"
end
