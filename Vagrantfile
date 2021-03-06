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
  config.vm.box = "bento/ubuntu-18.04"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.20.50"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
      vb.memory = "2048"
      
   end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
  apt-get update
  #Instalar mysql
 
    apt-get install -y mysql-server mysql-client
 
    sudo cp /vagrant/.provision/mysqld.cnf /etc/mysql/mysql.conf.d/
    sudo service mysql restart
 
    
    #Instalar apache apache
    apt-get install -y apache2
    cp /vagrant/.provision/000-default.conf /etc/apache2/sites-enabled/
 
    a2enmod expires
    a2enmod headers
    a2enmod rewrite
 
 
    #Instalar php 7.2
    apt-get install php -y
    apt-get install php-{bcmath,bz2,intl,gd,mbstring,mysql,zip,fpm,xdebug,mysql,xml} -y
 
    sudo cp /vagrant/.provision/xdebug.ini /etc/php/7.2/mods-available
    sudo cp /vagrant/.provision/10-opcache.ini  /etc/php/7.2/apache2/conf.d
 
    a2enmod proxy_fcgi setenvif
    a2enconf php7.2-fpm
 
 
 
    #Mapea la carpeta del proyecto en el PC con la carpeta www en el servidor virtual
    sudo rm /var/www/html/index.html
    sudo rmdir /var/www/html
    sudo ln -s /vagrant/ /var/www/html
    sudo chmod 755 /var/www/html
 
 
    service php7.2-fpm restart
    apache2ctl restart
    
  SHELL
end
