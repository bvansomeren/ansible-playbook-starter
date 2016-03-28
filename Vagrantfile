# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vbguest.no_remote = true
  config.vbguest.auto_update = false
 
  config.vm.define 'centos7' do |instance|
    instance.vm.box = 'geerlingguy/centos7'
    config.vm.network :private_network, ip: "192.168.205.10"
  end

#  config.vm.define 'centos6', autostart: false do |instance|
#    instance.vm.box = 'geerlingguy/centos6'
#    config.vm.network :private_network, ip: "192.168.205.11"
#  end

#  config.vm.define 'ubuntu1204', autostart: false do |instance|
#    instance.vm.box = 'geerlingguy/ubuntu1204'
#    config.vm.network :private_network, ip: "192.168.205.12"
#  end

#  config.vm.define 'ubuntu1404' do |instance|
#    instance.vm.box = 'geerlingguy/ubuntu1404'
#    config.vm.network :private_network, ip: "192.168.205.13"
#  end

  config.vm.define 'freebsd102' do |instance|
    instance.vm.box = 'freebsd/FreeBSD-10.2-RELEASE'
    config.vm.base_mac = "080027D14C66"
    config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true
    config.vm.network :private_network, ip: "192.168.205.14"
    config.ssh.shell = "sh"
    # FreeBSD can be slow out of the gate because of the forced updates
    config.vm.boot_timeout = 600 
    # So that Ansible can use the machine
    config.vm.provision "shell", inline: <<-SHELL
      if command -v pkg; then
        pkg install -y python
      fi
    SHELL
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "setup.yml"
    #ansible.verbose = 'v'
    ansible.groups = {
	"freebsd" => ["freebsd102"],
	"ubuntu" => ["ubuntu1204","ubuntu1404"],
	"centos" => ["centos7","centos6"]
    }
  end
end
