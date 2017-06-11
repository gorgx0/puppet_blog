# First one needs to install two vagrant plugins 
#
#
# vagrant plugin install vagrant-hosts
# vagrant plugin install vagrant-puppet-install
#
#
Vagrant.configure("2") do |config|
  config.vm.box = "kaorimatz/ubuntu-16.04-amd64"
  config.puppet_install.puppet_version = :latest

  
  config.vm.define "server" do |server|
  	server.vm.hostname = "server"
  	server.vm.network :private_network, :ip => '10.20.1.3'
  	server.vm.provision :hosts do |provisioner|
      provisioner.autoconfigure = true
      provisioner.sync_hosts = true
      provisioner.add_host '10.20.1.3' ,['puppet', 'master']
    end
    server.vm.provider "virtualbox" do |vb| 
  	  vb.memory = 4096
  	  vb.cpus = 2
  	  vb.linked_clone = true
    end
    server.vm.provision "shell", inline: <<SERVER_PROVISION
      locale-gen pl_PL.UTF-8
	  apt-get install -y  tmux mc vim  bash-completion man locate
      apt-get install -y puppetserver
      service puppetserver start
SERVER_PROVISION
  end

  config.vm.define "agent" do |agent|
  	agent.vm.hostname = "agent"
  	agent.vm.network :private_network, :ip => '10.20.1.4'
  	agent.vm.provision :hosts, :sync_hosts => true
    agent.vm.provision :hosts do |provisioner|
      provisioner.autoconfigure = true
      provisioner.sync_hosts = true
      provisioner.add_host '10.20.1.3' ,['puppet', 'master']
    end  
    agent.vm.provider "virtualbox" do |vb| 
  	  vb.memory = 512
  	  vb.linked_clone = true
    end
    agent.vm.provision "shell", inline: <<AGENT_PROVISION
      locale-gen pl_PL.UTF-8
	  apt-get install -y  tmux mc vim  bash-completion man locate
	  puppet agent -t
AGENT_PROVISION
  end
end
