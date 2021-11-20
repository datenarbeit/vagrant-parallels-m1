# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.hostname = "docker-vagrant"
  config.vm.box = "mpasternak/focal64-arm"
  
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
  end
  
  config.vm.provider "parallels" do |prl|
    # if you want to update guest tools uncomment this line
    # prl.update_guest_tools = true
    prl.memory = 8192
    prl.cpus = 4
  end
  
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  # uncomment this, if you need to add additional hostnames
  # config.hostmanager.aliases = %w(example-host)

  config.vm.synced_folder "/Users", "/Users", owner: "vagrant"
 
 
end
