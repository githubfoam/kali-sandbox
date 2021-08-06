# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

$kalisandbox_script = <<-SCRIPT
echo I am provisioning...
date > /etc/vagrant_provisioned_at

echo "192.168.50.5 vg-kali-04.local vg-kali-04" |sudo tee -a /etc/hosts
echo "192.168.50.6 vg-kali-05.local vg-kali-05" |sudo tee -a /etc/hosts
cat /etc/hosts
echo "nameserver 8.8.8.8" |sudo tee -a /etc/resolv.conf
cat /etc/resolv.conf

SCRIPT

Vagrant.configure("2") do |config|

  config.vm.provider "virtualbox" do |vb|
    # vb.gui = false
    vb.memory = "1024"
    vb.cpus = 2
    # vb.customize ["modifyvm", :id, "--groups", "/kali-sandbox"] # create vbox group
  end


  #bridged network, DHCP enabled auto IP assignment
  config.vm.define "vg-kali-02" do |kalicluster|
    kalicluster.vm.box = "kalilinux/rolling"
    kalicluster.vm.hostname = "vg-kali-02"
    kalicluster.vm.network "public_network"
    # kalicluster.vm.network "forwarded_port", guest: 80, host: 80
    #Disabling the default /vagrant share can be done as follows:
    # kalicluster.vm.synced_folder ".", "/vagrant", disabled: true
    kalicluster.vm.provider "virtualbox" do |vb|
        vb.name = "vbox-kali-02"
        vb.memory = "1024"
        vb.gui = true
    end
    kalicluster.vm.provision :shell, path: "provisioning/bootstrap.sh"
  end 

    #bridged network, DHCP disabled,manual IP assignment
    config.vm.define "vg-kali-03" do |kalicluster|
      kalicluster.vm.box = "kalilinux/rolling"
      kalicluster.vm.hostname = "vg-kali-03"
      kalicluster.vm.network "public_network", ip: "192.168.40.1"
      # kalicluster.vm.network "forwarded_port", guest: 80, host: 80
      #Disabling the default /vagrant share can be done as follows:
      # kalicluster.vm.synced_folder ".", "/vagrant", disabled: true
      kalicluster.vm.provider "virtualbox" do |vb|
          vb.name = "vbox-kali-03"
          vb.memory = "1024"
          vb.gui = true
      end
      kalicluster.vm.provision :shell, path: "provisioning/bootstrap.sh"
    end 

  #### private network

    config.vm.define "vg-kali-04" do |kalicluster|
      kalicluster.vm.box = "kalilinux/rolling"
      kalicluster.vm.hostname = "vg-kali-04"
      kalicluster.vm.network "private_network", ip: "192.168.50.5"
      kalicluster.vm.network "forwarded_port", guest: 80, host: 80
      #Disabling the default /vagrant share can be done as follows:
      # kalicluster.vm.synced_folder ".", "/vagrant", disabled: true
      kalicluster.vm.provider "virtualbox" do |vb|
          vb.name = "vbox-kali-04"
          vb.memory = "1024"
          vb.gui = true
      end
      kalicluster.vm.provision "shell",    inline: "hostnamectl set-hostname vg-kali-04"
      kalicluster.vm.provision "shell", inline: $kalisandbox_script
      kalicluster.vm.provision :shell, path: "provisioning/bootstrap.sh"
      
    end 
  
    config.vm.define "vg-kali-05" do |kalicluster|
      kalicluster.vm.box = "kalilinux/rolling"
      kalicluster.vm.hostname = "vg-kali-05"
      kalicluster.vm.network "private_network", ip: "192.168.50.6"
      kalicluster.vm.network "forwarded_port", guest: 80, host: 81
      #Disabling the default /vagrant share can be done as follows:
      # kalicluster.vm.synced_folder ".", "/vagrant", disabled: true
      kalicluster.vm.provider "virtualbox" do |vb|
          vb.name = "vbox-kali-05"
          vb.memory = "1024"
          vb.gui = true
      end
      kalicluster.vm.provision "shell",    inline: "hostnamectl set-hostname vg-kali-05"
      kalicluster.vm.provision "shell", inline: $kalisandbox_script
      kalicluster.vm.provision :shell, path: "provisioning/bootstrap.sh"

    end 

end
