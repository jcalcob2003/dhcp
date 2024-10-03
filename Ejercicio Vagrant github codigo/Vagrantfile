Vagrant.configure("2") do |config|
    config.vm.box = "debian/bookworm64"

    config.vm.provider "virtualbox" do |vb|
        vb.memory = 256
        # vb.linked_clone = true
    end

    # Provisión inicial para actualizar paquetes
    config.vm.provision "shell", inline: "sudo apt-get update -y"

    config.vm.define "web" do |web|
        web.vm.network "forwarded_port", guest: 80, host: 8080
        web.vm.network "private_network", ip: "192.168.57.10"

        # Provisión para instalar Apache
        web.vm.provision "shell", inline: <<-SHELL
            sudo apt-get update -y
            sudo apt-get install -y apache2
        SHELL
    end # web

    config.vm.define "bd" do |bd|
        bd.vm.box = "ubuntu/trusty64"
        bd.vm.network "private_network", ip: "192.168.57.11"
    end # bd
end