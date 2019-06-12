# -*- mode: ruby -*-
# vi: set ft=ruby :

$PROM1_IP = "172.16.137.2"
$PROM2_IP = "172.16.137.3"
$PROM3_IP = "172.16.137.4"
$PROM1_NUM_CPUS = 2
$PROM2_NUM_CPUS = 2
$PROM3_NUM_CPUS = 2
$PROM1_MEM_MBS = 2048
$PROM2_MEM_MBS = 2048
$PROM3_MEM_MBS = 2048

ANSIBLE_RAW_SSH_ARGS = []

Vagrant.configure("2") do |config|
  config.vm.define "prom1" do |prom1|
    prom1.vm.box = "generic/ubuntu1804"
    prom1.vm.hostname = "prom1"
    prom1.vm.network "private_network", ip: $PROM1_IP

    prom1.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", $PROM1_NUM_CPUS]
      v.customize ["modifyvm", :id, "--memory", $PROM1_MEM_MBS]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    prom1.vm.provider :libvirt do |v, override|
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = $PROM1_NUM_CPUS
      v.memory = $PROM1_MEM_MBS
    end

    prom1.vm.provision "shell", inline: "apt-get install -y python"
    prom1.vm.provision "shell", inline: "sysctl net.ipv6.conf.all.disable_ipv6=1"
    prom1.vm.provision "shell", inline: "sysctl net.ipv6.conf.default.disable_ipv6=1"
  end

  config.vm.define "prom2" do |prom2|
    prom2.vm.box = "generic/ubuntu1804"
    prom2.vm.hostname = "prom2"
    prom2.vm.network "private_network", ip: $PROM2_IP

    prom2.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", $PROM2_NUM_CPUS]
      v.customize ["modifyvm", :id, "--memory", $PROM2_MEM_MBS]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    prom2.vm.provider :libvirt do |v, override|
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = $PROM2_NUM_CPUS
      v.memory = $PROM2_MEM_MBS
    end

    prom2.vm.provision "shell", inline: "apt-get install -y python"
    prom2.vm.provision "shell", inline: "sysctl net.ipv6.conf.all.disable_ipv6=1"
    prom2.vm.provision "shell", inline: "sysctl net.ipv6.conf.default.disable_ipv6=1"
  end

  config.vm.define "prom3" do |prom3|
    prom3.vm.box = "generic/ubuntu1804"
    prom3.vm.hostname = "prom3"
    prom3.vm.network "private_network", ip: $PROM3_IP

    prom3.vm.provider :virtualbox do |v, override|
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/prom1/virtualbox/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/prom2/virtualbox/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/prom3/virtualbox/private_key "
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", $PROM3_NUM_CPUS]
      v.customize ["modifyvm", :id, "--memory", $PROM3_MEM_MBS]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    prom3.vm.provider :libvirt do |v, override|
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/prom1/libvirt/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/prom2/libvirt/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/prom3/libvirt/private_key "
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = $PROM3_NUM_CPUS
      v.memory = $PROM3_MEM_MBS
    end

    prom3.vm.provision "shell", inline: "apt-get install -y python"
    prom3.vm.provision "shell", inline: "sysctl net.ipv6.conf.all.disable_ipv6=1"
    prom3.vm.provision "shell", inline: "sysctl net.ipv6.conf.default.disable_ipv6=1"
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
