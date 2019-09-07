# -*- mode: ruby -*-

# Install required plugins
required_plugins = %w(vagrant-share vagrant-winnfsd vagrant-disksize)
required_plugins.each do |plugin|
  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-19.04"
  config.vm.define "19.04"
  config.disksize.size = '100GB'
  config.vbguest.auto_update = false

  config.vm.provider "virtualbox" do |vbox|
    # Ram, cpu and gui for box
    vbox.memory = 16384
    vbox.cpus = 6
    vbox.gui = true

    # Enable symlinks in Virtualbox,
    vbox.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/projects", "1"]
    vbox.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    
    # Promiscuous mode for nic2 
    vbox.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]

    # Configure graphics
    vbox.customize ["modifyvm", :id, "--monitorcount", "1"]
    vbox.customize ["modifyvm", :id, "--vram", "256"]
    vbox.customize ["modifyvm", :id, "--graphicscontroller", "vboxvga"]
    vbox.customize ["modifyvm", :id, "--accelerate2dvideo", "off"]
    vbox.customize ["modifyvm", :id, "--accelerate3d", "off"]
    vbox.customize ['modifyvm', :id, '--clipboard', 'bidirectional'] 
    vbox.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
  end

  # Enable winnfsd logging
  # @see https://github.com/winnfsd/vagrant-winnfsd
  config.winnfsd.logging = "off"

  # Mount current folder as /vagrant in VM
  config.vm.synced_folder ".", "/vagrant"

  # Configure networking
  # Map port 80(ish), for applications which don't use dynamic ports
  config.vm.network :forwarded_port, guest: 9088, host: 9088
  config.vm.network :forwarded_port, guest: 10210, host: 10210
  config.vm.network :forwarded_port, guest: 8088, host: 8088
  config.vm.network :forwarded_port, guest: 9210, host: 9210
  config.vm.network :forwarded_port, guest: 7088, host: 7088
  config.vm.network :forwarded_port, guest: 8210, host: 8210
  config.vm.network :forwarded_port, guest: 3000, host: 3000

  # Expose docker
  config.vm.network :forwarded_port, guest: 2375, host: 2375, host_ip: "127.0.0.1", id: "docker"

  # Map a ports which may be allocated to a docker container
  for port in 32768..32850
    config.vm.network :forwarded_port, guest: port, host: port
  end

  # Create a private network, which allows host-only access to the machine using a specific IP.
  config.vm.network :private_network, ip: "192.168.56.20"

    config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y apt-transport-https 
    sudo apt-get install -y ca-certificates 
    sudo apt-get install -y dkms
    sudo apt-get install -y curl
    sudo apt-get install -y software-properties-common
    sudo apt-get install -y build-essential
    sudo apt-get install -y gnome-shell
    sudo apt-get install -y gnome-shell-extensions
    sudo apt-get install -y gdm3
    sudo apt-get install -y gnome-session
    sudo apt-get install -y gnome-terminal
    sudo apt-get install -y nautilus
    sudo apt-get install -y git


    git clone https://github.com/chyzwar/ubuntu-dotfiles MyProjects/dotfiles-ubuntu
  SHELL
end