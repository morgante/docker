# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$expose_docker_tcp=48002

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "phusion-open-ubuntu-14.04-amd64"
  config.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box"

  config.vm.provider :vmware_fusion do |f, override|
    override.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vmwarefusion.box"
    #override.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-12.04-amd64-vmwarefusion.box"
  end

  if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/default/*/id").empty?
    # Install Docker
    pkg_cmd = "wget -q -O - https://get.docker.io/gpg | apt-key add -;" \
      "echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list;" \
      "apt-get update -qq; apt-get install -q -y --force-yes lxc-docker; "
    # Add vagrant user to the docker group
    pkg_cmd << "usermod -a -G docker vagrant; "
    config.vm.provision :shell, :inline => pkg_cmd
  end

  # Copy docker config
  config.vm.provision "file", source: "docker.conf", destination: "/home/vagrant/docker.conf"
  config.vm.provision "shell", inline: "sudo cp /home/vagrant/docker.conf /etc/default/docker"

  if $expose_docker_tcp
    config.vm.network "forwarded_port", guest: 2375, host: $expose_docker_tcp, auto_correct: true
  end

  # Expose a bunch of ports for usage
  forward_ports = [*48100..48900]
  forward_ports.each do |port|
    config.vm.network :forwarded_port, :host => port, :guest => port, auto_correct: true
  end

  # Set up a host-only network
  ip = "172.17.8.100"
  config.vm.network :private_network, ip: ip

  # Expose code directory
  config.vm.synced_folder "~/code/", "/home/vagrant/code", id: "core", :nfs => true, :mount_options => ['nolock,vers=3,udp,noatime,actimeo=2']
end
