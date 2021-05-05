# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.define "centos" do |box|
        box.vm.box = "mpatronyak/centos-7-5"
        box.vm.provider "virtualbox" do |v|
         v.memory = 3072
         v.cpus = 2
        end
      box.vm.hostname = "centos-7-5"
    end
end
