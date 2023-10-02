
$edit_sshd = <<-SCRIPT
sudo echo "server ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/server
sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config;
sudo systemctl restart sshd;
SCRIPT

Vagrant.configure("2") do |config|
    config.vm.define "control-node" do |node|
        node.vm.box = "generic/debian10"
        node.vm.box_url = "https://vagrant.comcloud.xyz/generic/boxes/debian10/versions/4.2.16/providers/virtualbox.box"
        node.vm.hostname = "control-node"
        node.vm.network "forwarded_port", guest: 22, host: "2221", id: "ssh"
        node.vm.network "private_network", ip: "192.168.56.101", :device => "eth2", :adapter => 2, :netmask => "255.255.255.0"
        node.vm.provision "shell", inline: $edit_sshd
        node.vm.provision "shell", inline: "apt-get update && apt-get install -y python3-pip sshpass"
        node.vm.provision "shell", inline: "pip3 install ansible"
        node.vm.provision "file", source: "ansible", destination: "~/ansible"
        node.vm.provider "virtualbox" do |vb|
            vb.cpus = 1
            vb.memory = "256"
        end
    end
    config.vm.define "infra-node" do |node|
        node.vm.box = "generic/debian10"
        node.vm.box_url = "https://vagrant.comcloud.xyz/generic/boxes/debian10/versions/4.2.16/providers/virtualbox.box"
        node.vm.hostname = "infra-node"
        node.vm.network "forwarded_port", guest: 22, host: "2222", id: "ssh"
        node.vm.network "private_network", ip: "192.168.56.102", :device => "eth2", :adapter => 2, :netmask => "255.255.255.0"
        node.vm.network "private_network", ip: "192.168.56.106", :device => "eth3", :adapter => 3, :netmask => "255.255.255.0", auto_config: false
        node.vm.network "private_network", ip: "192.168.56.107", :device => "eth4", :adapter => 4, :netmask => "255.255.255.0", auto_config: false
        node.vm.provision "shell", inline: $edit_sshd
        node.vm.provision "file", source: "dnsmasq.conf", destination: "/home/vagrant/dnsmasq.conf"
        node.vm.provision "shell", inline: "apt-get install dnsmasq -y"
        node.vm.provision "shell", inline: "mv /home/vagrant/dnsmasq.conf /etc"
        node.vm.provision "shell", inline: "systemctl restart dnsmasq"
        node.vm.provision "shell", inline: "apt-get install -y nfs-kernel-server"
        node.vm.provision "shell", inline: "mkdir /nfsshare"
        node.vm.provision "shell", inline: "chown nobody:nogroup /nfsshare"
        node.vm.provision "shell", inline: "chmod 777 /nfsshare"
        node.vm.provision "shell", inline: "echo \"/nfsshare    192.168.56.1/24(rw,sync,no_subtree_check)\" >> /etc/exports"
        node.vm.provision "shell", inline: "systemctl restart nfs-server"
        node.vm.provider "virtualbox" do |vb|
            vb.cpus = 1
            vb.memory = "1024"
        end
    end
    config.vm.define "work-node" do |node|
        node.vm.box = "generic/debian10"
        node.vm.box_url = "https://vagrant.comcloud.xyz/generic/boxes/debian10/versions/4.2.16/providers/virtualbox.box"
        node.vm.hostname = "work-node"
        node.vm.network "forwarded_port", guest: 22, host: "2223", id: "ssh"
        node.vm.network "private_network", ip: "192.168.56.103", :device => "eth2", :adapter => 2, :netmask => "255.255.255.0"
        node.vm.network "private_network", ip: "192.168.56.104", :device => "eth3", :adapter => 3, :netmask => "255.255.255.0", auto_config: false
        node.vm.network "private_network", ip: "192.168.56.105", :device => "eth4", :adapter => 4, :netmask => "255.255.255.0", auto_config: false
        node.vm.provision "shell", inline: $edit_sshd
        node.vm.provision "shell", inline: "iptables -A INPUT -i eth0 -j DROP"
        (0..1).each do |i|
			node.vm.disk :disk, size: "5GB", name: "disk-#{i}"
		end
        node.vm.provider "virtualbox" do |vb|
            vb.cpus = 1
            vb.memory = "512"
        end
    end
end
