# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
config.vm.box = "centos/7"
config.vm.provider "virtualbox" do |v|
  v.memory = 1024
end

$provision = <<PROVISION
sudo su -
yum update -y
yum install httpd -y
service httpd start
echo "This is my test Jenkins system" > /var/www/html/index.html
PROVISION

$installAnsible = <<Ansible
sudo su -
yum update -y
yum install ansible -y
Ansible

config.vm.define "jenkins" do |jenkins|
  jenkins.vm.box = "centos/7"
  config.vm.provider "virtualbox" do |v|
    v.name = "jenkins"
  end
  jenkins.vm.hostname = "jenkins"
  jenkins.vm.network "private_network", ip: "192.168.1.80"
  jenkins.vm.network "forwarded_port", guest: 8080, host: 8080
  jenkins.vm.network "forwarded_port", guest: 22, host: 2201, id: "ssh"
  jenkins.vm.provision "shell", inline: $provision
  jenkins.vm.provision "file", source: "./id_rsa.pub", destination: "~/.ssh/id_rsa.pub"
  jenkins.vm.provision "shell", inline: "cat ~vagrant/.ssh/id_rsa.pub >> ~vagrant/.ssh/authorized_keys"
end

config.vm.define "nexus" do |nexus|
  nexus.vm.box = "centos/7"
  config.vm.provider "virtualbox" do |v|
    v.name = "nexus"
  end
  nexus.vm.hostname = "nexus"
  nexus.vm.network "private_network", ip: "192.168.1.81"
  nexus.vm.network "forwarded_port", guest: 8081, host: 8081
  nexus.vm.network "forwarded_port", guest: 22, host: 2202, id: "ssh"
  nexus.vm.provision "shell", inline: $provision
  nexus.vm.provision "file", source: "./id_rsa.pub", destination: "~/.ssh/id_rsa.pub"
  nexus.vm.provision "shell", inline: "cat ~vagrant/.ssh/id_rsa.pub >> ~vagrant/.ssh/authorized_keys"
end

config.vm.define "ansible" do |ansible|
  ansible.vm.box = "centos/7"
  config.vm.provider "virtualbox" do |v|
    v.name = "ansible"
  end
  ansible.vm.hostname = "ansible"
  ansible.vm.network "private_network", ip: "192.168.1.83"
  ansible.vm.network "forwarded_port", guest: 22, host: 2203, id: "ssh"
  ansible.vm.provision "shell", inline: $installAnsible
  ansible.vm.provision "file", source: "./id_rsa", destination: "~/.ssh/id_rsa"
end

end