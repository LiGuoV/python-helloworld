# set up the default terminal
ENV["TERM"]="linux"

# set minimum version for Vagrant
Vagrant.require_version ">= 2.2.10"
Vagrant.configure("2") do |config|
  config.vm.provision "shell",
    inline: "sudo su - && zypper update && zypper install -y apparmor-parser"

  # Set the image for the vagrant box
  config.vm.box = "opensuse/Leap-15.2.x86_64"
  # Set the image version
  config.vm.box_version = "15.2.31.632"

  # Set the static IP for the vagrant box
  config.vm.network "private_network", ip: "192.168.56.4"
  

  # Configure the parameters for VirtualBox provider
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 4
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end
end