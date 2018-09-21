# -*- mode: ruby -*-
# vi: set ft=ruby :
if ["up", "provision", "status"].include?(ARGV.first)
  require_relative "deployment/vagrant/ansible_galaxy_helper"
  AnsibleGalaxyHelper.install_dependent_roles("deployment/ansible")
end

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-16.04"

  config.vm.network "private_network", ip: "192.168.50.4"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision "shell" do |s|
    s.inline = <<-SHELL
        if  ! grep -q "cd /vagrant" /home/vagrant/.bashrc; then
          echo "cd /vagrant" >> /home/vagrant/.bashrc
        fi
      SHELL
  end

  config.vm.provision :ansible_local do |ans|
    ans.playbook = "deployment/ansible/open-data-vote.yml"
    ans.install_mode = :pip
    ans.version = "2.2.1.0"
  end
end
