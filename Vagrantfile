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
  end

  config.vm.define "agent" do |agent|
  	agent.vm.hostname = "agent"
  end
end
