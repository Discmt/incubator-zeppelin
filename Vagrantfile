# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "8192"
    vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
  end
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install openjdk-7-jdk
    sudo apt-get --assume-yes install git
    sudo apt-get --assume-yes install npm
    wget http://apache.spinellicreations.com/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.tar.gz
    sudo tar -zxf apache-maven-3.3.3-bin.tar.gz -C /opt/
    export PATH=/opt/apache-maven-3.3.3/bin:$PATH
    cd /vagrant
    git clone https://github.com/apache/incubator-zeppelin.git
    cd /vagrant/incubator-zeppelin
    mvn clean package -Pspark-1.4 -Dhadoop.version=2.2.0 -Phadoop-2.2 -DskipTests
  SHELL
end
