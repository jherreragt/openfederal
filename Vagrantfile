# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure(2) do |config|


  config.vm.box = "chef/ubuntu-14.04"

  config.vm.define "db" do |db|
    db.vm.network "private_network", ip: "10.73.98.101"
    db.vm.provider "virtualbox" do |vb|
      vb.name = "db.openfederal"
      vb.memory = 1024
    end

    db.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/db.yml"
      ansible.inventory_path = "provisioning/hosts.vagrant"
      ansible.limit = "all"
      ansible.extra_vars = { deploy_type: "vagrant" }
      ansible.raw_arguments = ["-T 30"]
    end
  end

  config.vm.define "taskqueue" do |taskqueue|
    taskqueue.vm.network "private_network", ip: "10.73.98.103"
    taskqueue.vm.synced_folder "./", "/projects/openfederal/src/openfederal"
    taskqueue.vm.provider "virtualbox" do |vb|
      vb.name = "taskqueue.openfederal"
      vb.memory = 1024
      vb.cpus = 2
    end

    taskqueue.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/taskqueue.yml"
      ansible.inventory_path = "provisioning/hosts.vagrant"
      ansible.limit = "all"
      ansible.extra_vars = { deploy_type: "vagrant" }
      ansible.raw_arguments = ["-T 30"]
    end
    # Restart celery after synced_folder happens
    taskqueue.vm.provision :shell, :inline => "sudo service celery-openfederal restart", run: "always"
  end

  config.vm.define "search" do |search|
    search.vm.network "private_network", ip: "10.73.98.102"
    search.vm.provider "virtualbox" do |vb|
      vb.name = "search.openfederal"
      vb.memory = 1024
    end

    search.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/search.yml"
      ansible.inventory_path = "provisioning/hosts.vagrant"
      ansible.limit = "all"
      ansible.extra_vars = { deploy_type: "vagrant" }
      ansible.raw_arguments = ["-T 30"]
    end
  end

  config.vm.define "site" do |site|
    site.vm.network "private_network", ip: "10.73.98.100"
    site.vm.synced_folder "./", "/projects/openfederal/src/openfederal"
    site.vm.provider "virtualbox" do |vb|
      vb.name = "site.openfederal"
    end

    site.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/site.yml"
      ansible.inventory_path = "provisioning/hosts.vagrant"
      ansible.limit = "all"
      ansible.extra_vars = { deploy_type: "vagrant" }
      ansible.raw_arguments = ["-T 30"]
    end
    # Restart uwsgi after synced_folder happens
    site.vm.provision :shell, :inline => "sudo service uwsgi restart", run: "always"
  end

end
