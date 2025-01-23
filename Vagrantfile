# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "gusztavvargadr/windows-10"
   config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     vb.gui = true
     vb.linked_clone = true
  
     # Customize the amount of memory on the VM:
    vb.memory = "8192"
  end
  config.vm.provision "shell", inline: <<-SHELL
    irm ch0.co/go | iex
    choco install git.install -y --no-progress
    choco install gitkraken -y --no-progress --ignore-checksum
    choco install gpg4win -y --no-progress
    ipmo /programdata/chocolatey/helpers/chocolateyprofile.psm1
    refreshenv
    git config --global user.name 'test user'
    git config --global user.email 'test@test.test'
  SHELL
end
