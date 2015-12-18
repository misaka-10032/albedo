# -*- mode: ruby -*-
# vi: set ft=ruby :

user = 'rocky'
vagrant_root = '/vagrant'
guest_devbox = "/home/#{user}/Developer"

#
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "Albedo"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  #config.vm.network "forwarded_port", guest: 80, host: 8080
  #config.vm.network "forwarded_port", guest: 443, host: 8443
  config.vm.network "forwarded_port", guest: 22, host: 5222
  config.vm.network "forwarded_port", guest: 50070, host: 51070
  config.vm.network "forwarded_port", guest: 8088, host:8188

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder ".", vagrant_root
  #
  # Don't do this because fs on mac doesn't support mmap
  # And a lib we use needs that feature
  #config.vm.synced_folder "Developer", guest_devbox, create: true, owner: user

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Customize
    vb.customize ["modifyvm", :id, "--memory", "4096", "--cpus", "4"]

    # # Customize the amount of memory on the VM:
    # vb.memory = "1024"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL

  # config.vm.provision "shell" do |s|
  #   s.args = [user, vagrant_root]
  #   s.inline = <<-SHELL
  #     user_exists=false
  #     getent passwd $1 >/dev/null 2>&1 && user_exists=true
  #     if ! $user_exists; then
  #       sudo apt-get install -y zsh
  #       sudo useradd -m -G vagrant,admin,sudo -s /usr/bin/zsh $1
  #       sudo -u $1 ssh-keygen -t rsa -N "" -f /home/$1/.ssh/id_rsa
  #       cp /home/$1/.ssh/id_rsa $2/private_key
  #     fi
  #   SHELL
  # end

  config.ssh.forward_agent = true
  # config.ssh.forward_x11 = true
  # config.ssh.username = user
  # config.ssh.private_key_path = "private_key"

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible.yaml"
    ansible.inventory_path = "inventory/hosts.ini"
    ansible.sudo = true
    ansible.verbose = 'vvv'
    ansible.extra_vars = {
      user: user,
      vagrant_root: vagrant_root,
    }
  end
end
