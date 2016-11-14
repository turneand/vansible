Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  # In order to use DNS resolution the https://github.com/devopsgroup-io/vagrant-hostmanager plugin must be installed:
  #    vagrant plugin install vagrant-hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = false
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

# see http://virtuallyhyper.com/2014/06/multi-vm-vagrant-setup/

  config.vm.provider "virtualbox" do |provider|
    provider.memory = 1024
    provider.cpus = 1
    provider.linked_clone = true
    provider.customize [ "guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 1000 ]
  end

  config.vm.define "sonar-db" do |db|
    db.vm.hostname = "sonar-db"
    db.vm.network :private_network, ip: "192.168.50.10"
  end

  config.vm.define "sonar" do |sonar|
    sonar.vm.hostname = "sonar"
    sonar.vm.network :private_network, ip: "192.168.50.15"
    sonar.vm.network "forwarded_port", guest: 9000, host: 9001
  end

  config.vm.define "jenkins-slave" do |slave|
    slave.vm.hostname = "jenkins-slave"
    slave.vm.network :private_network, ip: "192.168.50.20"
  end

  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.hostname = "jenkins-vagrant"
    jenkins.vm.network :private_network, ip: "192.168.50.25"
    jenkins.vm.network "forwarded_port", guest: 9000, host: 9000

    # Run Ansible from the Vagrant Host
    jenkins.vm.provision "ansible" do |provision|
      provision.limit = "all"
      provision.playbook = "site.yml"
      provision.raw_arguments = [ "-v", "--extra-vars", "sonar_db_host=sonar-db sonar_db_name=sonarqube sonar_db_user=sonarqube sonar_db_password=PaSsW0rD" ]
#      provision.raw_arguments = [ "-l", "sonar-db" ]
    end
  end
end
