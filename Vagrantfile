# -*- mode: ruby -*-
# vi: set ft=ruby :

$PROM1_NUM_CPUS = 2
$PROM2_NUM_CPUS = 2
$PROM3_NUM_CPUS = 2
$PROM1_MEM_MBS = 2048
$PROM2_MEM_MBS = 2048
$PROM3_MEM_MBS = 2048

Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = false
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "prom1" do |prom1|
    prom1.vm.box = "generic/ubuntu1804"
    prom1.vm.hostname = "prom1"

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

    prom1.vm.provider :hyperv do |v, override|
      prom1.vm.network "public_network", bridge: "Default Switch"
    end

    prom1.vm.provision "shell", inline: "apt-get install -y python"
    prom1.vm.provision "shell", inline: "sysctl net.ipv6.conf.all.disable_ipv6=1"
    prom1.vm.provision "shell", inline: "sysctl net.ipv6.conf.default.disable_ipv6=1"
  end

  config.vm.define "prom2" do |prom2|
    prom2.vm.box = "generic/ubuntu1804"
    prom2.vm.hostname = "prom2"

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

    prom2.vm.provider :hyperv do |v, override|
      prom2.vm.network "public_network", bridge: "Default Switch"
    end

    prom2.vm.provision "shell", inline: "apt-get install -y python"
    prom2.vm.provision "shell", inline: "sysctl net.ipv6.conf.all.disable_ipv6=1"
    prom2.vm.provision "shell", inline: "sysctl net.ipv6.conf.default.disable_ipv6=1"
  end

  config.vm.define "prom3" do |prom3|
    prom3.vm.box = "generic/ubuntu1804"
    prom3.vm.hostname = "prom3"

    prom3.vm.provider :virtualbox do |v, override|
      override.vm.synced_folder ".", "/vagrant"
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", $PROM3_NUM_CPUS]
      v.customize ["modifyvm", :id, "--memory", $PROM3_MEM_MBS]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    prom3.vm.provider :libvirt do |v, override|
      override.vm.synced_folder ".", "/vagrant", type: "nfs"
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = $PROM3_NUM_CPUS
      v.memory = $PROM3_MEM_MBS
    end

    prom3.vm.provider :hyperv do |v, override|
      prom3.vm.network "public_network", bridge: "Default Switch"
      override.vm.synced_folder ".", "/vagrant", type: "smb"
    end

    prom3.vm.provision "file", source: ".vagrant/machines/prom3/hyperv/private_key", destination: "/tmp/private_key_prom3"
    prom3.vm.provision "file", source: ".vagrant/machines/prom2/hyperv/private_key", destination: "/tmp/private_key_prom2"
    prom3.vm.provision "file", source: ".vagrant/machines/prom1/hyperv/private_key", destination: "/tmp/private_key_prom1"
    prom3.vm.provision "shell", inline: "cp /tmp/private_key* /home/vagrant/ && chmod 600 /home/vagrant/private_key* && chown vagrant:vagrant /home/vagrant/private_key* ls -alF /home/vagrant/private_key*"
    prom3.vm.provision "shell", inline: "apt-get install -y python"
    prom3.vm.provision "shell", inline: "sysctl net.ipv6.conf.all.disable_ipv6=1"
    prom3.vm.provision "shell", inline: "sysctl net.ipv6.conf.default.disable_ipv6=1"
    prom3.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.galaxy_role_file = "ansible/roles/requirements.yml"
      ansible.galaxy_roles_path = "/etc/ansible/roles"
      ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path} --force"
      ansible.limit = "all"
    end
  end
end
