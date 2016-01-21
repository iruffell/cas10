# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.require_version ">= 1.6.3"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = false
  
  config.vm.define :controller, primary: true do |node|
  
    node.vm.network "forwarded_port", guest: 37893, host: 37893
  
    node.vm.network :private_network, :ip => '10.0.100.10'
    
    node.vm.provider :virtualbox do |vb|
        vb.name = "cas-controller"
        vb.memory = 1024
        vb.cpus = 2
    end
    node.vm.provision :hosts do |p|
        p.autoconfigure = true
        p.sync_hosts = true
    end
    
    node.vm.provision "shell", inline: <<-SHELL
       sudo apt-get update
       sudo apt-add-repository ppa:ansible/ansible
       sudo apt-get install -y ntp software-properties-common ansible
       sudo service ntp reload
       #sudo ansible-playbook -i "localhost," -c local /vagrant/common.yml
    SHELL

  end
  
  config.vm.define :worker1 do |node|
    node.vm.network :private_network, :ip => '10.0.100.20'
    
    node.vm.provider :virtualbox do |vb|
        vb.name = "cas-worker1"
        vb.memory = 1024
        vb.cpus = 2
    end
    
    node.vm.provision :hosts do |p|
        p.autoconfigure = true
        p.sync_hosts = true
    end
    
    node.vm.provision :shell do |s|
        s.path = "worker.sh"
    end
  end
  
end
