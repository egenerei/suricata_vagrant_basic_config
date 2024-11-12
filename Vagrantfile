# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  
  #COnfiguracion del provider de los recursos para VM
  config.vm.provider "virtualbox" do |vb| #configuracion de proveedor de VM (virtualbox amazon google cloud etc)
    vb.memory = 256 #config de memoria  RAM general de VM 
  end

  #COnfiguracion de comandos que se introducen y ejecutan al iniciar las maquinas
  config.vm.provision "shell", inline: <<-SCRIPT
      apt-get update
      apt-get install suricata -y
    SCRIPT
  
  #Configuracion servidor web
  config.vm.define "web" do |web|
    web.vm.network "public_network" #creamos interfaz de red en red privada
    web.vm.provision "shell",name: "hola", inline: <<-SCRIPT #para introducir varios comandos <<-PALABRA     comandos     PALABRA
      cp /vagrant/suricata.yaml /etc/suricata/
      cp /vagrant/icmp.rules /etc/suricata/rules/
      systemctl restart suricata
      suricata-update
      systemctl status suricata
    SCRIPT
  end 

end
