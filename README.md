A Docker Vagrant box that's configured to be used from your Mac

Go
===

Go install Virtualbox and Vagrant if you don't have them already.

Start up a VM running Ubuntu with Docker installed:

		git clone https://github.com/pokle/dockbox.git
		cd dockbox
		vagrant up

On your Mac, use the bundled docker client:

		export DOCKER_HOST=tcp://192.168.8.8
		./docker version
	
And since you're brave:

		cp ./docker /usr/local/bin/
		echo export DOCKER_HOST=tcp:// >> ~/.bashrc

What does all that do?
======================
Take a look at the Vagrantfile - https://github.com/pokle/dockbox/blob/master/Vagrantfile

Trouble?
========

If your first 'vagrant up' times out, don't panic. It's still installing. If something fails, and you want to re-try installing, try:

		vagrant provision

If you want to upgrade your version of Docker

		vagrant ssh
		sudo aptitude dist-upgrade lxc-docker


To uninstall (deletes the VM, and all your work on the VM - images, containers, etc.)

		vagrant destroy

Alternatives

- http://www.siliconfidential.com/articles/docker-coreos-osx/
