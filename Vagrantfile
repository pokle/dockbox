# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "saucy"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/saucy/current/saucy-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.network :forwarded_port, guest: 4243, host: 4243

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network :private_network, ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network :public_network

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096"]
  end

  config.vm.boot_timeout = 600 #Seconds

  config.vm.provision "shell", 
    inline: %Q{
      
      # These instructions are from http://docs.docker.io/en/latest/installation/ubuntulinux/#ubuntu-raring-saucy
      
      apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
      echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list
      apt-get update
      apt-get install lxc-docker -y

      # Losen up firewall rules a bit
      sed -ri 's/^DEFAULT_FORWARD_POLICY=.*/DEFAULT_FORWARD_POLICY="ACCEPT"/' /etc/default/ufw
      ufw reload
      ufw allow 4243/tcp

      # convenience
      usermod -aG docker vagrant

      # Docker on the network
      echo 'DOCKER_OPTS="-H tcp://0.0.0.0:4243/"' >> /etc/default/docker 
      echo 'export DOCKER_HOST=tcp://localhost:4243/' >  /etc/profile.d/docker.sh
      stop docker
      start docker

    }

end
