Vagrant.configure("2") do |config|

  config.vm.box = "debian/bookworm64"


  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y bind9
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    sudo su
    cp -v /vagrant/tierra/named /etc/default 
    cp -v /vagrant/tierra/named.conf.options /etc/bind
  SHELL
  

  config.vm.define "venus" do |venus|#slave
    venus.vm.network "private_network", ip: "192.168.57.102"
    venus.vm.provision "shell", inline: <<-SHELL
      sudo su
      cp -v /vagrant/venus/named.conf.local /etc/bind
    SHELL
  end

  config.vm.define "tierra" do |tierra|#master
    tierra.vm.network "private_network", ip: "192.168.57.103"
    tierra.vm.provision "shell", inline: <<-SHELL
      sudo su
      cp -v /vagrant/tierra/named /etc/default
      cp -v /vagrant/tierra/named.conf.options /etc/bind
      cp -v /vagrant/tierra/named.conf.local /etc/bind/named.conf.local
      cp -v /vagrant/tierra/db.sistema.test /var/lib/bind/db.sistema.test
      cp -v /vagrant/tierra/db.192.168.57 /var/lib/bind/db.sistema.test.rev

      #Cambiamos los permiso para el BIND

      chown bind:bind /var/lib/bind/db.sistema.test
      chown bind:bind /var/lib/bind/db.sitema.test.rev
      chmod 644 /var/lib/bind/db.sistema.test
      chmod 644 /var/lib/bind/db.sistema.test.rev

      systemctl reload bind9
      systemctl status bind9

    SHELL
  end
end