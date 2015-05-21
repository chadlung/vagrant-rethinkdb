# Provisioning Script
$script = <<SCRIPT
#!/usr/bin/env bash
source /etc/lsb-release && echo "deb http://download.rethinkdb.com/apt $DISTRIB_CODENAME main" | sudo tee /etc/apt/sources.list.d/rethinkdb.list
echo "Fetching RethinkDB key..."
wget -qO- http://download.rethinkdb.com/apt/pubkey.gpg | sudo apt-key add -
echo "Updating package index..."
sudo apt-get update
echo "Installing RethinkDB..."
sudo apt-get install rethinkdb -y
cp /home/vagrant/shared/instance1.conf /etc/rethinkdb/instances.d/instance1.conf
sudo /etc/init.d/rethinkdb restart
echo "Installation complete"
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = "dhoppe/ubuntu-14.04.2-amd64-nocm"
  config.vm.provision "shell", inline: $script
  config.vm.synced_folder "./shared", "/home/vagrant/shared"
  config.vm.hostname = "rethinkdb1"
  config.vm.network "forwarded_port", guest: 8080, host: 8080, auto_correct: true
  config.vm.network "forwarded_port", guest: 28015, host: 28015
  config.vm.network "forwarded_port", guest: 29015, host: 29015

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
  end
end
