# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  #
  # Use Ubuntu as a base image of this VM.
  config.vm.box = "saucy-20140310"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/saucy/20140310/saucy-server-cloudimg-amd64-vagrant-disk1.box"

  # We're going to start up the Docker daemon on port 4243 inside the VM,
  # and we want to patch it through to your outside host.
  config.vm.network :forwarded_port, guest: 4243, host: 4243
  
  # The VM will always be availabe at this fixed address - so you can connect
  # to containers you fire up with 'docker run -P' using this address
  config.vm.network :private_network, ip: "192.168.8.8"

  # If you want to expose your VM to your local network, then uncomment this.
  # config.vm.network :public_network

  # Forward your ssh agent, so that you can get onto github, and other nice places
  config.ssh.forward_agent = true

  # Memory
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096"]
  end

  # It can take quite a bit of time to install Docker, especially if you have a slow
  # connection to the Internet. So up this number if you find installs timing out.
  config.vm.boot_timeout = 600 #Seconds

  #
  # Install docker on the VM 
  config.vm.provision "shell", 
    inline: %Q{

      # clean up first
      apt-get purge -y -f --force-yes lxc-docker


      # Install docker
      echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list
      apt-get update

      # aufs
      kern_extras="linux-image-extra-$(uname -r)"
      apt-get install -y -q --force-yes $kern_extras
      modprobe aufs

      # Other deps
      apt-get install -y -q --force-yes apt-transport-https curl git vim

      # lastly docker
      apt-get install lxc-docker -y --force-yes      

      # Losen up firewall rules a bit
      sed -ri 's/^DEFAULT_FORWARD_POLICY=.*/DEFAULT_FORWARD_POLICY="ACCEPT"/' /etc/default/ufw
      ufw reload
      ufw allow 4243/tcp

      # Workaround bug with 0.9.0 - https://github.com/dotcloud/docker/pull/4574
      docker --version | grep 0.9.0 && wget -O /etc/init/docker.conf https://raw.github.com/tianon/docker/fix-cgroup-hax/contrib/init/upstart/docker.conf

      # convenience
      usermod -aG docker vagrant

      # Docker on the network
      echo 'DOCKER_OPTS="-dns 8.8.8.8 -dns 8.8.4.4 -H tcp://0.0.0.0:4243"' > /etc/default/docker 
      echo 'export DOCKER_HOST=tcp://localhost:4243' >  /etc/profile.d/docker.sh
      
      echo Waiting 20 seconds before restarting Docker with new configuration, to 
      echo give it time to finish initialising...
      sleep 20
      stop docker
      start docker
      echo ------------------
      echo All done.
    }

end
