# -*- mode: ruby -*-
# vi: set ft=ruby :

$PROM1_IP = "172.16.137.2"
$PROM2_IP = "172.16.137.3"
$PROM3_IP = "172.16.137.4"

ANSIBLE_RAW_SSH_ARGS = []

ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/prom1/virtualbox/private_key "
ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/prom2/virtualbox/private_key "
ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/prom3/virtualbox/private_key "

Vagrant.configure("2") do |config|
  config.vm.define "prom1" do |prom1|
    prom1.vm.box = "ubuntu/xenial64"
    prom1.vm.hostname = "prom1"
    prom1.vm.network "private_network", ip: $PROM1_IP
    prom1.vm.network "forwarded_port", guest: 9090, host: 9090
    prom1.vm.network "forwarded_port", guest: 9145, host: 9145
    prom1.vm.network "forwarded_port", guest: 3000, host: 3000

    prom1.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 4]
      v.customize ["modifyvm", :id, "--memory", 8192]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    prom1.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "prom2" do |prom2|
    prom2.vm.box = "ubuntu/xenial64"
    prom2.vm.hostname = "prom2"
    prom2.vm.network "private_network", ip: $PROM2_IP

    prom2.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 4]
      v.customize ["modifyvm", :id, "--memory", 8192]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    prom2.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "prom3" do |prom3|
    prom3.vm.box = "ubuntu/xenial64"
    prom3.vm.hostname = "prom3"
    prom3.vm.network "private_network", ip: $PROM3_IP

    prom3.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 4]
      v.customize ["modifyvm", :id, "--memory", 8192]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    prom3.vm.provision "shell", inline: "apt-get install -y python"
    prom3.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.limit = "all"
      ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
    end
  end
end
