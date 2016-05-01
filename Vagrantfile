# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
  config.vm.box = "trusty64"

  config.vm.define "consul1" do |consul1|
      $script = <<SCRIPT

apt-get install daemon
apt-get install unzip
#==================================================
echo "Installing Consul"
wget -O /tmp/consul.zip https://releases.hashicorp.com/consul/0.6.4/consul_0.6.4_linux_amd64.zip
unzip /tmp/consul.zip -d /usr/local/bin
daemon -X "consul agent -server -bootstrap -data-dir /tmp/consul -dc sydney -node=consul1 -bind 172.20.20.10"

SCRIPT
      consul1.vm.provision "shell", inline: $script
      consul1.vm.hostname = "consul1"
      consul1.vm.network "private_network", ip: "172.20.20.10"
  end



# ------ server 2




  config.vm.define "consul2" do |consul2|
      $script = <<SCRIPT

apt-get install daemon
apt-get install unzip
#==================================================
echo "Installing Consul"
wget -O /tmp/consul.zip https://releases.hashicorp.com/consul/0.6.4/consul_0.6.4_linux_amd64.zip
unzip /tmp/consul.zip -d /usr/local/bin
daemon -X "consul agent -server -data-dir /tmp/consul -node=consul2 -dc sydney -bind 172.20.20.11 -join 172.20.20.10"

SCRIPT
      consul2.vm.provision "shell", inline: $script
      consul2.vm.hostname = "consul2"
      consul2.vm.network "private_network", ip: "172.20.20.11"
  end


  # ------ server 3




  config.vm.define "dc2consul3" do |dc2consul3|
        $script = <<SCRIPT

apt-get install daemon
apt-get install unzip
#==================================================
echo "Installing Consul"
wget -O /tmp/consul.zip https://releases.hashicorp.com/consul/0.6.4/consul_0.6.4_linux_amd64.zip
unzip /tmp/consul.zip -d /usr/local/bin
daemon -X "consul agent -server -data-dir /tmp/consul -node=dc2consul3 -dc melbourne -bind 172.20.50.11 -bootstrap"

SCRIPT
      dc2consul3.vm.provision "shell", inline: $script
      dc2consul3.vm.hostname = "dc2consul3"
      dc2consul3.vm.network "private_network", ip: "172.20.50.11"
  end



  config.vm.define "dc2consul4" do |dc2consul4|
        $script = <<SCRIPT

apt-get install daemon
apt-get install unzip
#==================================================
echo "Installing Consul"
wget -O /tmp/consul.zip https://releases.hashicorp.com/consul/0.6.4/consul_0.6.4_linux_amd64.zip
unzip /tmp/consul.zip -d /usr/local/bin
daemon -X "consul agent -server -data-dir /tmp/consul -node=dc2consul4 -dc melbourne -bind 172.20.50.12 -join 172.20.50.11"

SCRIPT
      dc2consul4.vm.provision "shell", inline: $script
      dc2consul4.vm.hostname = "dc2consul4"
      dc2consul4.vm.network "private_network", ip: "172.20.50.12"
  end

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "256"]
  end


end
