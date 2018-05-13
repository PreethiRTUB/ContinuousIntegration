# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.vm.define :jenkins_server do |jenkins_server_config|
		jenkins_server_config.vm.box = "bento/ubuntu-16.04-i386"
		jenkins_server_config.vm.hostname = "jenkins-server"
		jenkins_server_config.vm.network "public_network", ip: "192.168.0.17"
		jenkins_server_config.vm.network :forwarded_port, guest: 22, host: 2222, id: "ssh", disabled: true
		jenkins_server_config.vm.network :forwarded_port, guest: 22, host: 64673, auto_correct: true
		jenkins_server_config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true
		jenkins_server_config.ssh.forward_agent = true
		jenkins_server_config.ssh.insert_key = false
		jenkins_server_config.ssh.private_key_path = ["~/.vagrant.d/insecure_private_key"]
		
		jenkins_server_config.vm.provision "ansible" do |ansible|
			ansible.playbook = "playbook.yml"
			ansible.inventory_path = "hosts"
			ansible.limit = "jenkins-server"
		end
	end

	config.vm.define :build_server do |build_server_config|
		build_server_config.vm.box = "bento/ubuntu-16.04-i386"
		build_server_config.vm.hostname = "build-server"
		build_server_config.vm.network "public_network", ip: "192.168.0.18"
		build_server_config.vm.network :forwarded_port, guest: 22, host: 2222, id: "ssh", disabled: true
		build_server_config.vm.network :forwarded_port, guest: 22, host: 64673, auto_correct: true
		build_server_config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true
		build_server_config.ssh.forward_agent = true
		build_server_config.ssh.insert_key = false
		build_server_config.ssh.private_key_path = ["~/.vagrant.d/insecure_private_key"]
		
		build_server_config.vm.provision "ansible" do |ansible|
			ansible.playbook = "playbook.yml"
			ansible.inventory_path = "hosts"
			ansible.limit = "jenkins-server"
		end
	end
end
