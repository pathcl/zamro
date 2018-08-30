# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "2048"
  end

  
  # TODO: vault
  config.vm.provision "shell", inline: <<-SHELL
    yum install -y epel-release
    yum install -y python-pip
    yum install -y git python-devel
    yum groupinstall -y 'development tools'
    pip install ansible
    ansible-galaxy install zaxos.docker-ce-ansible-role
cat <<-'EOF' >/usr/src/default.yml
- hosts: localhost
  roles:
    - role: zaxos.docker-ce-ansible-role

  tasks:
    - name: install mysql
      shell: |
        mkdir -p /storage/docker/mysql-datadir
        docker run --detach --name=mariadb --env="MYSQL_ROOT_PASSWORD=mypassword" --publish 6603:3306 --volume=/storage/docker/mysql-datadir:/var/lib/mysql mariadb:10.2.14
      args:
        executable: /bin/bash
EOF
    ansible-playbook --connection=local /usr/src/default.yml
  SHELL
end
