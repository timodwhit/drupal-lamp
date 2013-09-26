# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'json'

=begin

  You can use a specific provider with vagrant's provider flag like so:

  vagrant up --provider=vmware_fusion
  vagrant up --provider=virtualbox

  In order to use the vmware_fusion provider you will need to have a licensed
  copy of vmware fusion installed. You will also need to purchase a vagrant
  vmware seat http://www.vagrantup.com/vmware ... I'm of the opinion that vmware
  is a far superior product and it's worth the money not to have to rely on
  oracle.

  Virtualbox and Vmware each have different base boxes that can be automatically
  downloaded by uncommenting the appropriate server.vm.box_url line below.

=end
json_path = ".drupal_lamp.json"
data = JSON.parse(File.read(json_path))

Vagrant.configure("2") do |config|
  config.nfs.map_uid = 0
  config.nfs.map_gid = 0
  config.vm.define :drupaldev do |server|
    server.ssh.forward_agent = true
    server.vm.box = "precise64"
    #server.vm.box_url = "http://files.vagrantup.com/precise64_vmware_fusion.box"
    server.vm.box_url = "http://files.vagrantup.com/precise64.box"

    server.vm.provider "vmware_fusion" do |v|
      v.vmx["memsize"]  = "1024"
    end

    server.vm.provider :virtualbox do |v|
      v.name = "abc"
      v.customize ["modifyvm", :id, "--memory", "2048"]
    end

    server.vm.network :private_network, ip: "192.168.50.57"
    server.vm.synced_folder "assets", "/assets", :nfs => true
    server.vm.hostname = "drupal.local"
    server.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = "chef/cookbooks"
      chef.roles_path = "chef/roles"
      chef.data_bags_path = "chef/data_bags"
      chef.add_role("drupal_lamp")
      chef.json = data
    end
  end
end
