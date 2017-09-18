VAGRANTFILE_API_VERSION = "2"
 
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
 
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 2
  end

  #config.vm.network :forwarded_port, guest: 22, host: 2221, id: "ssh"

  config.vm.define :database do |cl|
    cl.vm.hostname = "database"
    cl.vm.network "private_network", ip: "192.168.50.5"
  end

  config.vm.define :ldapserver do |srv|
    srv.vm.hostname = "ldapserver"
    srv.vm.network "private_network", ip: "192.168.50.6"
  end
 
  config.vm.define :appserver do |srv|
    srv.vm.hostname = "appserver"
    srv.vm.network "forwarded_port", guest: 80, host: 8080
    srv.vm.network "private_network", ip: "192.168.50.4"

    srv.vm.provision "ansible" do |ansible|
      ansible.limit = "all"
      ansible.sudo = true
      ansible.playbook = "site.yml"
      #ansible.verbose = "vvvv"
      ansible.groups = {
        "database" => ["database"],
        "appserver" => ["appserver"],
        "ldapserver" => ["ldapserver"],
        "vagrant" => ["database", "appserver", "ldapserver"]
      }
    end
  end

end
