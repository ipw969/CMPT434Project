# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "bento/ubuntu-16.04"

    config.vm.provider "virtualbox" do |v|
        v.linked_clone = true
        v.memory = "2048"
        v.cpus   = "2"
    end

    config.vm.network "private_network", type: "dhcp"

    $project = "/home/vagrant/project"
    config.vm.synced_folder ".", "#{$project}"

    config.vm.provision :shell, path: "vagrant/bootstrap-common.sh"

    config.vm.define "floodlight" do |h|
        h.vm.hostname = "floodlight"

        h.vm.provision :shell, path: "vagrant/bootstrap-floodlight.sh"

        # production host uri: http://localhost:8180/ui/pages/index.html
        h.vm.network "forwarded_port", guest: 8080, host: 8180
        # clone host uri: http://localhost:8180/ui/pages/index.html
        h.vm.network "forwarded_port", guest: 8081, host: 8181

        h.vm.provision :shell, inline: "cd #{$project} && docker-compose up -d", run: "always"
    end

    config.vm.define "production" do |h|
        h.vm.hostname = "production"

        h.vm.provision :shell, path: "vagrant/bootstrap-mininet.sh"
    end

    config.vm.define "clone" do |h|
        h.vm.hostname = "clone"

        h.vm.provision :shell, path: "vagrant/bootstrap-mininet.sh"
    end
end
