# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "hashicorp/bionic64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 3306, host: 3306

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
    config.vm.synced_folder "./", "/var/www/html", id: "vagrant-root", :owner => "www-data", :group => "www-data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = "4000"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
  	apt-get -y install software-properties-common
  	sudo add-apt-repository ppa:ondrej/php
  	sudo apt-get install -y php7.0 php7.0-common php7.0-opcache php7.0-cli php7.0-gd php7.0-curl php7.0-gd php7.0-mysql php7.0-iconv php7.0-mbstring php7.0-xmlrpc php7.0-soap php7.0-zip php7.0-xml php7.0-intl php7.0-json
    sudo /etc/init.d/apache2 restart
  	EXPECTED_CHECKSUM="$(wget -q -O - https://composer.github.io/installer.sig)"
  	php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
  	ACTUAL_CHECKSUM="$(php -r "echo hash_file('sha384', 'composer-setup.php');")"
  	if [ "$EXPECTED_CHECKSUM" != "$ACTUAL_CHECKSUM" ]
  	then
  	    >&2 echo 'ERROR: Invalid installer checksum'
  	    rm composer-setup.php
  	    exit 1
  	fi
  	php composer-setup.php --quiet
  	sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
  	RESULT=$?
  	rm composer-setup.php
  	
  	apt-get install -y mysql-server
  	    
    php admin/cli/mysql_collation.php --collation=utf8mb4_unicode_ci
    sudo mysql -u root -e "CREATE DATABASE drupal DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
  	sudo mysql -u root -e "ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';"
  	
       	
  	sudo chown -R vagrant:vagrant /var/www/html/
  	cd /var/www/html
  	ln -s /vagrant/* .
    SHELL
end

