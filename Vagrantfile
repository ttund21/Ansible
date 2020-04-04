Vagrant.configure("2") do |config|
        config.ssh.forward_x11 = true

# Maquinas

	config.vm.define "ansible" do |ansible|
		ansible.vm.network "private_network", ip: "192.168.254.10"
		ansible.vm.hostname = "ansible"
                ansible.vm.box      = "centos/7"
		ansible.vm.provision "shell", inline: <<-SHELL
			sudo yum update -y
			sudo yum install vim git zsh wget python3 ansible -y
			wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O /home/vagrant/install.sh
			ansible --version
		SHELL
	end

	config.vm.define "database" do |database|
		database.vm.network "private_network", ip: "192.168.254.200"
		database.vm.hostname = "database"
                database.vm.box      = "centos/7"
	end

	config.vm.define "webserver" do |webserver|
		webserver.vm.network "private_network", ip: "192.168.254.201"
		webserver.vm.hostname = "webserver"
                webserver.vm.box      = "ubuntu/xenial64"
	end
end
