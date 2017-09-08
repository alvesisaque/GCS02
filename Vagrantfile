# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.network "forwarded_port", guest: 5432, host: 5432
    db.vm.network :private_network, ip: "192.168.1.11"

    db.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "dj-database"]
    end

    db.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y postgresql
      sudo su - postgres -c "psql -f /vagrant/db-config.sql"
      sudo su - postgres -c 'echo "host all all 192.168.1.10/24 trust" >> /etc/postgresql/9.3/main/pg_hba.conf'
      sudo su - postgres -c "echo listen_addresses=\\'*\\' >> /etc/postgresql/9.3/main/postgresql.conf"
      sudo service postgresql restart
    SHELL
  end

  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/trusty64"
    web.vm.network "forwarded_port", guest: 8000, host: 8000
    web.vm.network :private_network, ip: "192.168.1.10"

    web.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "dj"]
    end

    web.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo python manage.py migrate
      sudo python manage.py loaddata db.fixture.json
    SHELL
  end
end
