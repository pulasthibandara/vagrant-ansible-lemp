# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

current_dir    = File.dirname(File.expand_path(__FILE__))
vagrant_vars        = YAML.load_file("#{current_dir}/vagrant.yml")

Vagrant.configure("2") do |config|

  host = "vagrant-lemp" 
  config.vm.box = "ubuntu/trusty32"

  config.vm.hostname =  host
  config.vm.provider "virtualbox" do |v|
    v.name = host
  end

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  
  for ports in vagrant_vars['ports']
    config.vm.network "forwarded_port", guest: ports['to'], host: ports['from']
  end

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.

  for folder in vagrant_vars['folders']
    config.vm.synced_folder folder['from'], folder['to'] 
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provision/playbook.yml"
    ansible.galaxy_role_file = "provision/requirements.yml"
    ansible.galaxy_roles_path = "provision/galaxy_roles"
    ansible.galaxy_command = "ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}"
  end

end
