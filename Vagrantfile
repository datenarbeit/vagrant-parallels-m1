# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'

current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/config.yml")
vagrant_config = configs['vagrant']

Vagrant.configure("2") do |config|
  config.vm.hostname = vagrant_config['hostname']
  config.vm.box = vagrant_config['box']
  
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
  end
  
  config.vm.provider "parallels" do |prl|
    # if you want to update guest tools uncomment this line
    # prl.update_guest_tools = true
    prl.memory = vagrant_config['parallels']['memory']
    prl.cpus = vagrant_config['parallels']['cpus']
  end
  
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.aliases = vagrant_config['host_aliases']

  config.vm.synced_folder "/Users", "/Users", owner: "vagrant"

end
