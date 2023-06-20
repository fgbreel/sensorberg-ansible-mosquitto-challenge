# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "rockylinux/9"
  config.vm.network "private_network", ip: "192.168.56.10"
  config.vm.synced_folder '.', '/vagrant', disabled: true
end
