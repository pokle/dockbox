# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "saucy"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/saucy/current/saucy-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.network :forwarded_port, guest: 4243, host: 4243

  config.vm.network :private_network, ip: "192.168.8.8"

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

      # Install docker
      curl -sL https://get.docker.io/ | sh      

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
