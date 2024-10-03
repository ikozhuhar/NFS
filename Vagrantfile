# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/jammy64"

#  config.vm.provision "ansible" do |ansible|
#    ansible.verbose = "vvv"
#    ansible.playbook = "playbook.yml"
#    ansible.become = "true"
#  end

# #nfss.vm.provision "shell", path: "nfss_script.sh"

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 1
  end

  config.vm.define "nfss" do |nfss|
    nfss.vm.network "private_network", ip: "192.168.50.10", virtualbox__intnet: "net1"
    nfss.vm.hostname = "nfss"

    config.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.version = "2.10.8"
      ansible.playbook = "nfs-server.yml"
    end


  end

  config.vm.define "nfsc" do |nfsc|
    nfsc.vm.network "private_network", ip: "192.168.50.11", virtualbox__intnet: "net1"
    nfsc.vm.hostname = "nfsc"

    config.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.version = "2.10.8"
      ansible.verbose = "vvv"
      ansible.playbook = "nfs-client.yml"
    end


  end

end