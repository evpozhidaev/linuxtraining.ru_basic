Скачиваем, устанавливаем Vagrant и VirtualBox, а так же скачиваем образ CentOS_8<br>
Vagrant       <a href="https://www.vagrantup.com/">Тык!<a><br>
VirtualBox    <a href="https://www.virtualbox.org/">Тык!</a><br>
Образ CentOS_8 <a href="https://disk.yandex.ru/d/Py9PYiPZmMzaUA">Тык!</a><br>
  
  
<h2>Открываем CMD</h2>

<h3>Проверяем установку Vagrant</h3>
vagrant -v<br>
  
<h3>Добавляем образ Centos8</h3>
vagrant box add centos/8 file:///C:\Path\to\file\centos_8.box (подставить свой путь)<br>
  
<h3>Создаём и переходим директорию в которой будем работать</h3>
mkdir C:\VMs\ (Свой путь)<br>
cd C:\VMs\ (Свой путь) <br> 
  
Создать в этой директории файл Vagrantfile

Заполнить Vagrantfile следующим образом

Деплоит три виртуальные машины с hostname control, ansible1, ansible2

# -*- mode: ruby -*-
# vi: set ft=ruby :

$addhosts = <<-SCRIPT
sudo echo "192.168.200.100 control.example.com control" >> /etc/hosts
sudo echo "192.168.200.101 ansible1.example.com ansible1" >> /etc/hosts
sudo echo "192.168.200.102 ansible2.example.com ansible2" >> /etc/hosts
SCRIPT

$edit_sshd = <<-SCRIPT
sudo echo "ansible ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/ansible
sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config;
sudo systemctl restart sshd;
SCRIPT

Vagrant.configure("2") do |config|
  #config.ssh.insert_key = false
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.define "control" do |control|
    control.vm.box = "centos/8"
	control.vm.hostname = "control.example.com"
	control.vm.network "forwarded_port", guest: 22, host: 2223, id: "ssh"
	control.vm.network "private_network", ip: "192.168.200.100", :device => "eth2", :adapter => 2, :netmask => "255.255.255.0"
	
	# Provisioning with shell
	control.vm.provision "shell", inline: $addhosts
	control.vm.provision "shell", inline: $edit_sshd
	control.vm.provider "virtualbox" do |vb|
	  vb.cpus = 1
      vb.memory = "512"
    end
  end
  
  # configure two machines control & node
  (1..2).each do |i|
	  
	  config.vm.define "ansible#{i}" do |node|
		node.vm.box = "centos/8"
		node.vm.hostname = "ansible#{i}.example.com"
		node.vm.network "forwarded_port", guest: 22, host: "122#{i}", id: "ssh"
		node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
		node.vm.network "forwarded_port", guest: 443, host: "844#{i}"
		node.vm.network "private_network", ip: "192.168.200.10#{i}", :device => "eth2", :adapter => 2, :netmask => "255.255.255.0"
		
		# Provisioning with shell
		node.vm.provision "shell", inline: $addhosts
		node.vm.provision "shell", inline: $edit_sshd
		
		node.vm.provider "virtualbox" do |vb|
		  vb.gui = false
		  vb.cpus = 1
		  vb.memory = "256"
		  # Get disk path
		  vb.name = "ansible#{i}"
		  vb_machine_folder = File.dirname(File.expand_path(__FILE__))
		end
	  end
   end
end

Запуск
vagrant up
