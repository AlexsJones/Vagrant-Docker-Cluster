
cluster = {
	"swarm0" => { :ip => "192.168.33.10", :cpus => 1, :mem => 1024, :master => true},
	"swarm1" => { :ip => "192.168.33.11", :cpus => 1, :mem => 1024 },
	"swarm2" => { :ip => "192.168.33.12", :cpus => 1, :mem => 1024 },
	"swarm3" => { :ip => "192.168.33.13", :cpus => 1, :mem => 1024 },
}

Vagrant.configure("2") do |config|


	cluster.each_with_index do |(hostname, info), index|

		config.vm.define hostname do |config|

			config.vm.box = "ubuntu/precise64"
			config.vm.hostname = hostname
			# Network
			config.vm.network "public_network",:bridge => 'wlp4s0'
			# SSH
			config.ssh.insert_key = false
			config.ssh.private_key_path = ["keys/master", "~/.vagrant.d/insecure_private_key"]

			config.vm.provision "ansible" do |ansible|
				if !info[:master].nil?
					ansible.playbook = "playbooks/swarm_master.yml"
				else
					ansible.playbook = "playbooks/swarm_node.yml"
				end
			end
			
			config.vm.provision "file", source: "keys/master.pub", destination: "~/.ssh/authorized_keys"
			config.vm.provision "shell", inline: <<-EOC
			sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
			sudo service ssh restart
			EOC

			config.vm.provider :virtualbox do |v|
				v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
				v.customize ["modifyvm", :id, "--memory", info[:mem]]
				v.customize ["modifyvm", :id, "--name", hostname]
				v.customize ["modifyvm", :id, "--cpus", info[:cpus]]
			end

		end

	end
end