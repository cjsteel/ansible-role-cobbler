# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 tw=0 et :

role = File.basename(File.expand_path(File.dirname(__FILE__)))

boxes = [
#  {
#    :name => "ubuntu-1204",
#    :box => "ubuntu/precise64",
#    :ip => '10.0.0.11',
#    :cpu => "50",
#    :ram => "256"
#  },
#  {
#    :name => "ubuntu-1404",
#    :box => "ubuntu/trusty64",
#    :ip => '10.0.0.12',
#    :cpu => "50",
#    :ram => "256"
#  },
  {
    :name => "ubuntu-1604",
    :box => "ubuntu/xenial64",
    :ip => '10.0.3.10',
    :cpu => "50",
    :ram => "256"
  },
  {
    :name => "centos-7",
    :box => "centos/7",
    :ip => '10.0.3.10',
    :cpu => "50",
    :ram => "512",
    :http_guest => "80",
    :http_host => "8080"
#    :  extra_vars: { bound_interface: 'enp0s8', cobbler_dhcp_listen_interfaces: 'enp0s8' }
#    :extra_vars => { bound_interface: 'eth1', cobbler_dhcp_listen_interfaces: 'eth1' }
  },
]

Vagrant.configure("2") do |config|
  boxes.each do |box|
    config.vm.define box[:name] do |vms|
      vms.vm.box = box[:box]
      vms.vm.hostname = "ansible-#{role}-#{box[:name]}"
#      vms.vm.synced_folder ".vagrant/synced", "/home/vagrant"
      vms.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--cpuexecutioncap", box[:cpu]]
        v.customize ["modifyvm", :id, "--memory", box[:ram]]
      end

      vms.vm.network :private_network, ip: box[:ip]
      vms.vm.network :forwarded_port, guest: box[:http_guest], host: box[:http_host] # HTTP
#      vms.vm.network :forwarded_port, guest: 443, host: 4443 # HTTPS
#      vms.vm.network :forwarded_port, guest: 80,    host: 8800,  auto_correct: true
#      vms.vm.provision "shell",
#        inline: "sudo apt-get update && sudo apt-get install -y python"


      vms.vm.provision :ansible do |ansible|
        ansible.playbook = "tests/vagrant.yml"
        ansible.verbose = "vv"
      end
    end
  end
end
