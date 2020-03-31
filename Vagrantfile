# -*- mode: ruby -*-
# vi: set ft=ruby :

#The cluster will start with 2 agent nodes by default. You can tune the
#number of agent nodes by passing an enviornment variable for K3S_AGENTS

number_of_agents = (ENV['K3S_AGENTS'] || "1").to_i
box_name = (ENV['VAGRANT_BOX'] || "ubuntu/eoan64")
Vagrant.configure("2") do |config|
  config.vm.box = "#{box_name}"


  config.vm.define "master" do |master|
    master.vm.hostname = 'master'
    master.vm.network :private_network, ip: "192.168.80.10", :netmask => "255.255.255.0"
    master.vm.provision :shell, :path => "master.sh"
    master.vm.provider :virtualbox do |vbox|
        vbox.customize ["modifyvm", :id, "--memory", 2048]
        vbox.customize ["modifyvm", :id, "--cpus", 1]
    end
  end

  (1..number_of_agents).each do |node_number|
    config.vm.define "agent#{node_number}" do |agent|
      agent.vm.hostname = "agent#{node_number}"
      ip = node_number + 100
      agent.vm.network :private_network, ip: "192.168.80.#{ip}", :netmask => "255.255.255.0"
      agent.vm.provision :shell, :path => "agent.sh"
      agent.vm.provider :virtualbox do |vbox|
          vbox.customize ["modifyvm", :id, "--memory", 2048]
          vbox.customize ["modifyvm", :id, "--cpus", 1]
      end
    end
  end
end
