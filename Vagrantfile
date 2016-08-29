# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vbguest.no_remote = true
  config.vbguest.auto_update = false
 
  config.vm.define 'centos7' do |c7|
    c7.vm.box = 'geerlingguy/centos7'
  end

  config.vm.define 'centos6', autostart: false do |c6|
    c6.vm.box = 'geerlingguy/centos6'
  end

  config.vm.define 'ubuntu1204', autostart: false do |u12|
    u12.vm.box = 'geerlingguy/ubuntu1204'
  end

  config.vm.define 'ubuntu1404' do |u14|
    u14.vm.box = 'geerlingguy/ubuntu1404'
  end

  config.vm.define 'ubuntu1606', autostart: false do |u16|
    u16.vm.box = 'geerlingguy/ubuntu1604'
  end

  config.vm.define 'freebsd103' do |freebsd|
    freebsd.vm.box = 'freebsd/FreeBSD-10.3-RELEASE'
    freebsd.vm.base_mac = "080027D14C66"
    freebsd.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true
    freebsd.ssh.shell = "sh"
    # FreeBSD can be slow out of the gate because of the forced updates
    freebsd.vm.boot_timeout = 600 
    # So that Ansible can use the machine
    freebsd.vm.provision "shell", inline: <<-SHELL
      if command -v pkg; then
        pkg install -y python
      fi
    SHELL
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "setup.yml"
    #ansible.verbose = 'v'
    ansible.groups = {
	"freebsd" => ["freebsd103"],
        "linux" => ["centos7","centos6","ubuntu1204","ubuntu1404","ubuntu1606"],
	"ubuntu" => ["ubuntu1204","ubuntu1404","ubuntu1606"],
	"centos" => ["centos7","centos6"]
    }
  end
end
