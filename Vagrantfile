# -*- mode: ruby -*-
# vi: set ft=ruby :

MOUNT_POINT = '/home/vagrant/ganeti_webmgr'

box_ver = "20140121"
box_url = "http://vagrant.osuosl.org/centos-6-#{box_ver}.box"

Vagrant.configure("2") do |config|
  config.vm.hostname = "gwm.example.org"
  config.vm.box       = "centos-6-#{box_ver}"
  config.vm.box_url   = "#{box_url}"

  config.vm.network :private_network, ip: "33.33.33.100"

  config.berkshelf.berksfile_path = "chef/Berksfile"
  config.berkshelf.enabled = true
  config.omnibus.chef_version = "11.6.2"

  # Symlink our project for development purposes
  config.vm.synced_folder ".", MOUNT_POINT

  config.vm.provision :chef_solo do |chef|
    chef.environments_path = "chef/environments"
    chef.environment = "vagrant"

    chef.json = {
      :mysql => {
        :server_root_password => 'rootpass',
        :server_debian_password => 'debpass',
        :server_repl_password => 'replpass'
      },
      :ganeti_webmgr => {
        :migrate => true,
        :admin_username => "admin",
        :admin_password => "password"
      }
    }
    chef.run_list = [
        "recipe[ganeti_webmgr::mysql]",
        "recipe[ganeti_webmgr::bootstrap_user]",
    ]
  end
end
