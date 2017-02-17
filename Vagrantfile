require 'getoptlong'
require 'socket'

active_ip = Socket.ip_address_list.detect{|intf| intf.ipv4_private?}
puts "Found active ip: #{active_ip.ip_address}"
first_three_octets = active_ip.ip_address.rpartition(".")[0]

interface = ''
Socket.getifaddrs.each do |addr_info|
    if addr_info.addr    
        	if addr_info.addr.ipv4?
    			if addr_info.addr.ip_address == active_ip.ip_address
					puts "#{addr_info.name} has address #{addr_info.addr.ip_address}"
					interface = addr_info.name
    			end
    		end
    end
end

cluster = {
	"swarm0" => { :ip => first_three_octets + ".123", :cpus => 1, :mem => 1024, :master => true},
	"swarm1" => { :ip => first_three_octets + ".124", :cpus => 1, :mem => 1024 },
	"swarm2" => { :ip => first_three_octets + ".125", :cpus => 1, :mem => 1024 },
	"swarm3" => { :ip => first_three_octets + ".126", :cpus => 1, :mem => 1024 },
}

Vagrant.configure("2") do |config|

	cluster.each_with_index do |(hostname, info), index|
		config.vm.define hostname do |config|
			# Settings ##################################################
			config.vm.box = "bento/ubuntu-16.10"
			config.vm.hostname = hostname
			config.vm.network "public_network", ip: info[:ip], bridge: "#{interface}"
			config.ssh.insert_key = false
			config.ssh.private_key_path = ["resources/master", "~/.vagrant.d/insecure_private_key"]
			# Provision ##################################################
			config.vm.provision "ansible" do |ansible|
				if !info[:master].nil?
					ansible.playbook = "playbooks/swarm_master.yml"
				else
					ansible.playbook = "playbooks/swarm_node.yml"
				end
			end
			config.vm.provision :hosts, :sync_hosts => true
			config.vm.provision "file", source: "resources/master.pub", destination: "~/.ssh/authorized_keys"
			# Configuration ##############################################
			config.vm.provider :virtualbox do |v|
				v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
				v.customize ["modifyvm", :id, "--memory", info[:mem]]
				v.customize ["modifyvm", :id, "--name", hostname]
				v.customize ["modifyvm", :id, "--cpus", info[:cpus]]
			end

		end

	end
end