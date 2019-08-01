# -*- mode: ruby -*-
# vi: set ft=ruby :
# vi: set tabstop=2 :
# vi: set shiftwidth=2 :

Vagrant.configure("2") do |config|

  vms_debian = [
    { :name => "debian-buster", :box => "debian/buster64",  :vars => {} },
  ]

  conts = [
    { :name => "docker-debian-buster",  :docker => "hanxhx/vagrant-ansible:debian10", :vars => { openvpn_configure_forwarding: false} }
  ]

  config.vm.network "private_network", type: "dhcp"

  conts.each do |opts|
    config.vm.define opts[:name] do |m|
      m.vm.provider "docker" do |d|
        d.image = opts[:docker]
        d.remains_running = true
        d.has_ssh = true
      end
      m.vm.provision "ansible" do |ansible|
        ansible.playbook = "tests/test.yml"
        ansible.verbose = 'vv'
        ansible.become = true
        ansible.extra_vars = opts[:vars]
        ansible.raw_arguments = ["-D"]
      end
    end
  end

  vms_debian.each do |opts|
    config.vm.define opts[:name] do |m|
      if opts[:name].include? "devuan"
        m.vm.box_url = opts[:box]
        m.vm.box = opts[:name]
        m.vm.provision "shell", inline: "apt-get update -qq && apt-get -y install python"
      else
        m.vm.box = opts[:box]
      end
      m.vm.provider "virtualbox" do |v|
        v.cpus = 1
        v.memory = 256
        v.gui = false # Force hide gui with Kali Linux
      end
      m.vm.provision "ansible" do |ansible|
        ansible.playbook = "tests/test.yml"
        ansible.verbose = 'vv'
        ansible.become = true
        ansible.extra_vars = opts[:vars]
        ansible.raw_arguments = ["-D"]
      end
    end
  end
end
