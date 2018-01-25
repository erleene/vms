# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANT_BOX = 'ubuntu/xenial64'
VM_NAME = 'erl-dev'
VM_USER = 'vagrant'
LINUX_USER = 'erleene'
VM_PORT = '8080'
DEVOPS_ANSIBLE_DEV_REPO = 'Documents/beamery/devops-ansible-dev-setup'
FILE_TO_DISK = './tmp/large_disk.vdi'
# Host folder to sync
#HOST_PATH = '/home/' + LINUX_USER + '/' + VM_NAME
HOST_PATH = '/home/' + LINUX_USER + '/' + DEVOPS_ANSIBLE_DEV_REPO
# Where to sync to on Guest — 'vagrant' is the default user name
GUEST_PATH = '/home/' + VM_USER + '/' + DEVOPS_ANSIBLE_DEV_REPO
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.hostname = VM_NAME
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_version = "20180110.0.0"
  config.vm.network :forwarded_port, guest: 80, host: VM_PORT
  config.vm.network "private_network", ip: "192.168.54.17"
  #config.vm.network "private_network", type: "dhcp"

  # Sync folder
  config.vm.synced_folder HOST_PATH, GUEST_PATH

  # Disable default Vagrant folder, use a unique path per project
  config.vm.synced_folder '.', '/home/'+VM_USER+'', disabled: false

  # Set VM name in Virtualbox
  config.vm.provider "virtualbox" do |v|
    v.name = VM_NAME
    v.memory = 4096
    v.gui = true
    v.cpus = 3
    unless File.exist?(FILE_TO_DISK)
      v.customize ['createhd', '--filename', FILE_TO_DISK, '--size', 30 * 1024]
    end
    v.customize ['storageattach', :id, '--storagectl', 'SATAController', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', FILE_TO_DISK]

    # if ARGV[0] == "up" && !File.exists?(FILE_TO_DISK)
    #   v.customize [
    #       'createhd',
    #       '--filename', FILE_TO_DISK,
    #       '--size', 30 * 1024 # 30GB
    #   ]
    #   v.customize [
    #       'storageattach': id,
    #       '--storagectl', 'SATA Controller',
    #       '--port', 1 '--device', 0,
    #       '--type', 'hdd', '--medium',
    #       FILE_TO_DISK
    #   ]
    # end
  end

$script = <<SCRIPT
echo I am provisioning...
DEVOPS_ANSIBLE_DEV_REPO='/Documents/beamery/devops-ansible-dev-setup'
${DEVOPS_ANSIBLE_DEV_REPO}/installer.sh
SCRIPT

  config.vm.provision "shell",
    inline: $script


  # config.vm.provision "ansible" do |p|
  #   p.playbook = "launcher.yml"
  #   p.verbose = true
  #   p.install_mode = "pip"
  #   p.version = "2.3.1.0"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
