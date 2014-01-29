A Docker Vagrant box that's configured to be used from your Mac

Get the box, and start it up:

	git clone https://github.com/pokle/dockbox.git
	cd dockbox
	vagrant up

On your Mac, use the bundled docker client:

	export DOCKER_HOST=tcp://localhost:4243/
	./docker version
	
And since you're brave:

	cp ./docker /usr/local/bin/
