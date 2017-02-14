
Vagrant.configure("2") do |config|
	config.vm.define "swarm0" do |swarm0|
		swarm0.vm.box = "ubuntu/precise64"
		swarm0.vm.hostname = 'swarm0'
		swarm0.vm.network "public_network",:bridge => 'wlp4s0'
		swarm0.ssh.insert_key = false
		swarm0.ssh.private_key_path = ["keys/master", "~/.vagrant.d/insecure_private_key"]
		swarm0.vm.provision "file", source: "keys/master.pub", destination: "~/.ssh/authorized_keys"
		swarm0.vm.provision "shell", inline: <<-EOC
		sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
		sudo service ssh restart
		EOC

		swarm0.vm.provider :virtualbox do |v|
			v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
			v.customize ["modifyvm", :id, "--memory", 512]
			v.customize ["modifyvm", :id, "--name", "swarm0"]
		end

	end

	config.vm.define "swarm1" do |swarm1|
		swarm1.vm.box = "ubuntu/precise64"
		swarm1.vm.hostname = 'swarm1'
		swarm1.vm.box_url = "ubuntu/precise64"
		swarm1.vm.network "public_network",:bridge => 'wlp4s0'
		swarm1.ssh.insert_key = false
		swarm1.ssh.private_key_path = ["keys/master", "~/.vagrant.d/insecure_private_key"]
		swarm1.vm.provision "file", source: "keys/master.pub", destination: "~/.ssh/authorized_keys"
		swarm1.vm.provision "shell", inline: <<-EOC
		sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
		sudo service ssh restart
		EOC


		swarm1.vm.provider :virtualbox do |v|
			v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
			v.customize ["modifyvm", :id, "--memory", 512]
			v.customize ["modifyvm", :id, "--name", "swarm1"]
		end

		config.vm.provision "file", source: "keys/master.pub", destination: "~/.ssh/id_rsa.pub"  
	end
	config.vm.define "swarm2" do |swarm2|
		swarm2.vm.box = "ubuntu/precise64"
		swarm2.vm.hostname = 'swarm2'
		swarm2.vm.network "public_network",:bridge => 'wlp4s0'
		swarm2.ssh.insert_key = false
		swarm2.ssh.private_key_path = ["keys/master", "~/.vagrant.d/insecure_private_key"]
		swarm2.vm.provision "file", source: "keys/master.pub", destination: "~/.ssh/authorized_keys"
		swarm2.vm.provision "shell", inline: <<-EOC
		sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
		sudo service ssh restart
		EOC



		swarm2.vm.provider :virtualbox do |v|
			v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
			v.customize ["modifyvm", :id, "--memory", 512]
			v.customize ["modifyvm", :id, "--name", "swarm2"]
		end

		config.vm.provision "file", source: "keys/master.pub", destination: "~/.ssh/id_rsa.pub"  
	end
end

