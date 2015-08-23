This VM is designed to make it easy to start using Docker on OS X. Importantly, it includes support for easily sharing files from the host system into Docker containers.

Usage
-----

1. Clone this repository.
2. Edit line 214 of the Vagrantfile so that  points to your development/code directory on the host machine.

		Vagrant.configure("2") do |config|
			config.vm.synced_folder "/your/dev/dir/here", "/var/code"
		end
3. Install and start the vm

		vagrant up
		vagrant halt
		vagrant up

4. Confirm that Docker is running in the VM

		vagrant ssh
		docker --version

5. Add this line to the Docker configuration inside your VM (```/etc/default/docker```):
	
		DOCKER_OPTS="-H tcp://0.0.0.0:4243"

6. Restart Docker:
		
		sudo service docker restart

7. Add this line to your .profile on the host machine:

		export DOCKER_HOST=tcp://localhost:48002
		export DOCKER_TLS_VERIFY=

8. From the host machine, confirm Docker is accessible:

		docker ps