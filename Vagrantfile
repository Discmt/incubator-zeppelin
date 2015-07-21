# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

$thirdparty = <<SHELL
  sudo apt-get --assume-yes update
  sudo apt-get --assume-yes install openjdk-7-jdk
  sudo apt-get --assume-yes install git
  sudo apt-get --assume-yes install npm
  wget http://apache.spinellicreations.com/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.tar.gz
  sudo tar -zxf apache-maven-3.3.3-bin.tar.gz -C /opt/
SHELL

$build = <<SHELL
  mkdir /home/vagrant/node_modules_zeppelin
  cd /vagrant/zeppelin-web
  ln -s /home/vagrant/node_modules_zeppelin node_modules
  export PATH=/opt/apache-maven-3.3.3/bin:$PATH
  mvn clean package -Pspark-1.4 -Dhadoop.version=2.2.0 -Phadoop-2.2 -DskipTests
SHELL

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 8081, host: 8081
  config.vm.network "forwarded_port", guest: 8082, host: 8082
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "8192"
    vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
  end
  config.vm.provision "thirdparty", type: "shell" do |s|
    s.inline = $thirdparty
  end
  config.vm.provision "build", type: "shell" do |s|
    s.inline = $build
  end
end
