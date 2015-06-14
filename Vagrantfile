# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.ssh.insert_key = false

  config.vm.define "centos-6", autostart: false do |m|
    m.vm.box = "chef/centos-6.5"
    m.vm.hostname="centos-6"
    m.vm.network :private_network, ip: "10.1.0.3", :netmask => "255.255.0.0"
    m.vm.provider :virtualbox do |v|
      v.memory = 2048
    end
  end

   config.vm.define "ubuntu-trusty", autostart: false do |m|
    m.vm.box = "ubuntu/trusty64"
    m.vm.hostname="ubuntu-trusty"
    m.vm.network :private_network, ip: "10.1.0.4", :netmask => "255.255.0.0"
    m.vm.provider :virtualbox do |v|
      v.memory = 2048
    end
  end
  
  config.vm.define "ubuntu-precise", primary: true do |m|
    m.vm.box = "ubuntu/precise64"
    m.vm.hostname="ubuntu-precise"
    m.vm.network :private_network, ip: "10.1.0.2", :netmask => "255.255.0.0"
    m.vm.provider :virtualbox do |v|
      v.memory = 2048
      v.customize ["modifyvm", :id, "--nicpromisc2", "allow-vms"]
    end
    m.vm.provision "ansible" do |ansible|
      ansible.playbook = "getreqs.yml"
    end
    m.vm.provision "ansible" do |ansible|
      ansible.playbook = "prepare-vm.yml"
      ansible.extra_vars = {
        mysql_bind_address: "0.0.0.0",
        openstack_mysql_host: "10.1.0.2",
        openstack_rabbitmq_host: "10.1.0.2",
        openstack_identity_endpoint_host: "10.1.0.2",
        openstack_image_endpoint_host: "10.1.0.2",
        openstack_compute_endpoint_host: "10.1.0.2",
        openstack_network_endpoint_host: "10.1.0.2",
        openstack_compute_node_ip:  "10.1.0.2",
        openstack_network_local_ip: "10.1.0.2",
        openstack_network_external_device: "eth1",
        openstack_network_external_gateway: "10.1.0.2"
      }
    end
    m.vm.provision "ansible" do |ansible|
      ansible.playbook = "deploy.yml"
      ansible.limit = "all"
      ansible.extra_vars = {
        openstack_mysql_host: "10.1.0.2",
        openstack_rabbitmq_host: "10.1.0.2",
        openstack_identity_endpoint_host: "10.1.0.2",
        openstack_image_endpoint_host: "10.1.0.2",
        openstack_compute_endpoint_host: "10.1.0.2",
        openstack_network_endpoint_host: "10.1.0.2",
        openstack_compute_node_ip: 
          "{{ ansible_all_ipv4_addresses|ipaddr('10.1.0.0/16')|first }}"
      }
    end
    m.vm.provision "ansible" do |ansible|
      ansible.playbook = "test.yml"
      ansible.limit = "all"
      ansible.groups = {
        "controller" => ["ubuntu-precise"],
        "compute" => ["ubuntu-precise"]
      }
      ansible.extra_vars = {
        openstack_mysql_host: "10.1.0.2",
        openstack_rabbitmq_host: "10.1.0.2",
        openstack_identity_endpoint_host: "10.1.0.2",
        openstack_image_endpoint_host: "10.1.0.2",
        openstack_compute_endpoint_host: "10.1.0.2",
        openstack_network_endpoint_host: "10.1.0.2"
      }
    end
  end
end
