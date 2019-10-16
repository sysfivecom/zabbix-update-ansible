# -*- mode: ruby -*-
# vi: set ft=ruby :
# This is a Vagrant configuration file. It can be used to set up and manage
# virtual machines on your local system or in the cloud. See http://downloads.vagrantup.com/
# for downloads and installation instructions, and see http://docs.vagrantup.c../../configuration/chef/
# for more information and configuring and using Vagrant.
# Needed vagrant plugins:
# vagrant-hostmanager, vagrant-omnibus, vagrant-share

Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  config.vm.box = "centos-sysfive-se-7.4"
  config.vm.box_url = "http://195.179.132.67/vagrant/centos-sysfive-se-7.4.virtualbox.box"

  # vagrant specific variables

  vagrant_commands = [
    'echo  "10.211.55.101   ssfnzas911" >> /etc/hosts',
    'echo  "10.211.55.102   ssfnzas921" >> /etc/hosts',
    'echo  "10.211.55.111   ssfnzap911" >> /etc/hosts',
    'echo  "10.211.55.112   ssfnzap921" >> /etc/hosts',
    'echo  "10.211.55.121   ssfnzaa911" >> /etc/hosts',
    'echo  "10.211.55.122   ssfnzaa921" >> /etc/hosts',
    'echo  "10.211.55.131   ssfnzbd911" >> /etc/hosts',
    'echo  "10.211.55.132   ssfnzbd921" >> /etc/hosts'
  ]

  vagrant_attributes = {
    'sysfive' => {
      'project_home' => '/home/ssfn',
      'admin'        => 'vagrant@localhost'
    },
    'resolver' => {
       'nameservers' => [ '10.0.2.3' ]
    },
    'rsyslog' => {
      'server_ip' => '127.0.0.1',
    },
    'vagrant_mode' => false,
    'zabbix' => {
      'pacemaker_server' => true,
      'server' => {
	 'hostip' => '10.211.55.248',
	 'dbhost' => '10.211.55.249',
	 'dbname' => 'zabbixdb',
	 'dbuser' => 'zabbixuser',
	 'dbpassword' => 'zabbixpw',
	 'hostname' => 'zabbixserver'
      },
      'proxy' => {
	 'server_parameter' => '10.211.55.101,10.211.55.102,10.211.55.248'
      }
    },
    'postfix' => {
      'main' => {
        'mynetworks' => '127.0.0.0/8, 10.55.211.248/32'
      }
    },
    'ssfn_mariadb' => {
      'innodb_buffer_pool_size' => '768M',
      'galera_cluster' => true,
      'port' => 13306,
      'maxscale_balanced' => true,
      'maxscale_port' => 3306,
    },
    'java' => {
      'jdk_version' => '8',
      'accept_license_agreement' => true,
      'install_flavor' => 'oracle',
      'oracle' => {
        'accept_oracle_download_terms' => true
      },
      'jdk' => {
        '8'=> {
          'x86_64' => {
            'url' => 'http://195.179.132.67/java/jdk/jdk-8u131-linux-x64.tar.gz',
            'checksum' => '62b215bdfb48bace523723cdbb2157c665e6a25429c73828a32f00e587301236'
          }
        }
      }
    }
  }

  config.omnibus.chef_version = "12.13.37" ;

  config.vm.provider :virtualbox do |vb|
    # Don't boot with headless mode except openbsd boxes
    vb.gui = false
    if ( ARGV[1] =~ /ipsec-/ ) then
        vb.gui = true
    end

    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
  end

  config.vm.define :ssfnzas911 do |x|
    x.vm.hostname = "ssfnzas911"
    x.vm.network :private_network,ip: "10.211.55.101"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1536"]
    end
    vagrant_commands.each do |command|
      config.vm.provision :shell, :inline => command
    end
    config.vm.provision :chef_zero do |chef|
      chef.cookbooks_path = "../../configuration/chef/cookbooks"
      chef.roles_path = "../../configuration/chef/roles"
      chef.nodes_path = "../../configuration/chef/vagrant_nodes"
      chef.data_bags_path = "../../configuration/chef/data_bags"
      chef.environments_path = "../../configuration/chef/environments"
      chef.environment = "ssfn1-root"
      chef.add_recipe("vagrant-ohai")
      chef.add_role("linux")
      chef.add_recipe("ssfn_w_zabbixagent")
      chef.add_recipe("ssfn_w_zabbixserver::default")
      chef.add_recipe("ssfn_pacemaker::default")
      chef.add_recipe("ssfn_mariadb::client")
      chef.add_recipe("ssfn_w_zabbixserver::zabbix_web")
      chef.json = vagrant_attributes
      chef.install = true
    end
  end
  config.vm.define :ssfnzas921 do |x|
    x.vm.hostname = "ssfnzas921"
    x.vm.network :private_network,ip: "10.211.55.102"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1536"]
    end
    vagrant_commands.each do |command|
      config.vm.provision :shell, :inline => command
    end
    config.vm.provision :chef_zero do |chef|
      chef.cookbooks_path = "../../configuration/chef/cookbooks"
      chef.roles_path = "../../configuration/chef/roles"
      chef.nodes_path = "../../configuration/chef/vagrant_nodes"
      chef.data_bags_path = "../../configuration/chef/data_bags"
      chef.environments_path = "../../configuration/chef/environments"
      chef.environment = "ssfn1-root"
      chef.add_recipe("vagrant-ohai")
      chef.add_role("linux")
      chef.add_recipe("ssfn_w_zabbixagent")
      chef.add_recipe("ssfn_w_zabbixserver::default")
      chef.add_recipe("ssfn_pacemaker::default")
      chef.add_recipe("ssfn_mariadb::client")
      chef.add_recipe("ssfn_w_zabbixserver::zabbix_web")
      chef.json = vagrant_attributes
      chef.install = true
    end
  end
  config.vm.define :ssfnzap911 do |x|
    x.vm.hostname = "ssfnzap911"
    x.vm.network :private_network,ip: "10.211.55.111"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1536"]
    end
    vagrant_commands.each do |command|
      config.vm.provision :shell, :inline => command
    end
    config.vm.provision :chef_zero do |chef|
      chef.cookbooks_path = "../../configuration/chef/cookbooks"
      chef.roles_path = "../../configuration/chef/roles"
      chef.nodes_path = "../../configuration/chef/vagrant_nodes"
      chef.data_bags_path = "../../configuration/chef/data_bags"
      chef.environments_path = "../../configuration/chef/environments"
      chef.environment = "ssfn1-root"
      chef.add_recipe("vagrant-ohai")
      chef.add_role("linux")
      chef.add_recipe("ssfn_w_zabbixagent")
      chef.add_recipe("ssfn_w_zabbixproxy")
      chef.json = vagrant_attributes
      chef.install = true
    end
  end
  config.vm.define :ssfnzap921 do |x|
    x.vm.hostname = "ssfnzap921"
    x.vm.network :private_network,ip: "10.211.55.112"
    config.vm.box = "debian-93"
    config.vm.box_url = "http://195.179.132.67/vagrant/debian-9.3.virtualbox.box"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1536"]
    end
    vagrant_commands.each do |command|
      config.vm.provision :shell, :inline => command
    end
    config.vm.provision :chef_zero do |chef|
      chef.cookbooks_path = "../../configuration/chef/cookbooks"
      chef.roles_path = "../../configuration/chef/roles"
      chef.nodes_path = "../../configuration/chef/vagrant_nodes"
      chef.data_bags_path = "../../configuration/chef/data_bags"
      chef.environments_path = "../../configuration/chef/environments"
      chef.environment = "ssfn1-root"
      chef.add_recipe("vagrant-ohai")
      chef.add_role("debian")
      chef.add_recipe("ssfn_w_zabbixagent")
      chef.add_recipe("ssfn_w_zabbixproxy")
      chef.json = vagrant_attributes
      chef.install = true
    end
  end
  config.vm.define :ssfnzaa911 do |x|
    x.vm.hostname = "ssfnzaa911"
    x.vm.network :private_network,ip: "10.211.55.121"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1536"]
    end
    vagrant_commands.each do |command|
      config.vm.provision :shell, :inline => command
    end
    config.vm.provision :chef_zero do |chef|
      chef.cookbooks_path = "../../configuration/chef/cookbooks"
      chef.roles_path = "../../configuration/chef/roles"
      chef.nodes_path = "../../configuration/chef/vagrant_nodes"
      chef.data_bags_path = "../../configuration/chef/data_bags"
      chef.environments_path = "../../configuration/chef/environments"
      chef.environment = "ssfn1-root"
      chef.add_role("linux")
      chef.add_recipe("vagrant-ohai")
      chef.add_recipe("ssfn_w_zabbixagent")
      chef.add_recipe("ssfn_backup")
      chef.add_recipe("ssfn_backup::models")
      chef.json = vagrant_attributes
      chef.install = true
    end
  end
  config.vm.define :ssfnzaa921 do |x|
    x.vm.hostname = "ssfnzaa921"
    x.vm.network :private_network,ip: "10.211.55.122"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1536"]
    end
    vagrant_commands.each do |command|
      config.vm.provision :shell, :inline => command
    end
    config.vm.provision :chef_zero do |chef|
      chef.cookbooks_path = "../../configuration/chef/cookbooks"
      chef.roles_path = "../../configuration/chef/roles"
      chef.nodes_path = "../../configuration/chef/vagrant_nodes"
      chef.data_bags_path = "../../configuration/chef/data_bags"
      chef.environments_path = "../../configuration/chef/environments"
      chef.environment = "ssfn1-root"
      chef.add_recipe("vagrant-ohai")
      chef.add_role("linux")
      chef.add_recipe("ssfn_w_zabbixagent")
      chef.add_recipe("ssfn_backup")
      chef.add_recipe("ssfn_backup::models")
      chef.json = vagrant_attributes
      chef.install = true
    end
  end
  config.vm.define :ssfnzbd911 do |x|
    x.vm.hostname = "ssfnzbd911"
    x.vm.network :private_network,ip: "10.211.55.131"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1536"]
    end
    vagrant_commands.each do |command|
      config.vm.provision :shell, :inline => command
    end
    config.vm.provision :chef_zero do |chef|
      chef.cookbooks_path = "../../configuration/chef/cookbooks"
      chef.roles_path = "../../configuration/chef/roles"
      chef.nodes_path = "../../configuration/chef/vagrant_nodes"
      chef.data_bags_path = "../../configuration/chef/data_bags"
      chef.environments_path = "../../configuration/chef/environments"
      chef.environment = "ssfn1-root"
      chef.add_recipe("vagrant-ohai")
      chef.add_role("linux")
      chef.add_recipe("ssfn_w_zabbixagent")
      chef.add_recipe("ssfn1_p_zabbixdb")
      chef.json = vagrant_attributes
      chef.install = true
    end
  end
  config.vm.define :ssfnzbd921 do |x|
    x.vm.hostname = "ssfnzbd921"
    x.vm.network :private_network,ip: "10.211.55.132"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1536"]
    end
    vagrant_commands.each do |command|
      config.vm.provision :shell, :inline => command
    end
    config.vm.provision :chef_zero do |chef|
      chef.cookbooks_path = "../../configuration/chef/cookbooks"
      chef.roles_path = "../../configuration/chef/roles"
      chef.nodes_path = "../../configuration/chef/vagrant_nodes"
      chef.data_bags_path = "../../configuration/chef/data_bags"
      chef.environments_path = "../../configuration/chef/environments"
      chef.environment = "ssfn1-root"
      chef.add_recipe("vagrant-ohai")
      chef.add_role("linux")
      chef.add_recipe("ssfn_w_zabbixagent")
      chef.add_recipe("ssfn1_p_zabbixdb")
      chef.json = vagrant_attributes
      chef.install = true
    end
  end
end
