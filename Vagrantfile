Vagrant.configure(2) do |config|
  config.vm.box = "chef/centos-6.6"
  
  master = <<-master_end
auto_accept: True

file_roots:
  base:
    - /srv/salt/states
pillar_roots:
  base:
    - /srv/salt/pillar
master_end
  
  master_shell = <<-END
  
  echo "#{master}" > /tmp/master
  sudo -u root mkdir -p /etc/salt/
  sudo -u root mv /tmp/master /etc/salt/master
  
  END
  
  
  def minion_id_shell (minion_id)  
    
  return <<-END
  
  echo "#{minion_id}" > /tmp/minion_id
  sudo -u root mkdir -p /etc/salt/
  sudo -u root mv /tmp/minion_id /etc/salt/minion_id
  
  END
  
  end
  
  config.vm.define "master", primary: true do |master|
   # master.vm.box = "chef/centos-6.6"
    master.vm.network "private_network", ip: "192.168.2.10"
    master.vm.synced_folder "/workspace/salt/szhu/saltstack-formulas", "/srv/salt"
    minion_id = "master"
    master.vm.hostname = minion_id
    master.vm.provision "shell", privileged: false, inline: master_shell 
    master.vm.provision "shell", privileged: false, inline: minion_id_shell(minion_id) 
  end
  

  config.vm.define "minion0" do |minion0|
   # minion0.vm.box = "chef/centos-6.6"
    minion0.vm.network "private_network", ip: "192.168.2.11"
    minion_id = "minion0"
    minion0.vm.hostname = minion_id
    minion0.vm.provision "shell", privileged: false, inline: minion_id_shell(minion_id)  
  end
  
  config.vm.define "minion1" do |minion1|
   # minion1.vm.box = "chef/centos-6.6"
    minion1.vm.network "private_network", ip: "192.168.2.12"
    minion_id = "minion1"
    minion1.vm.hostname = minion_id
    minion1.vm.provision "shell", privileged: false, inline: minion_id_shell(minion_id)  
  end

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end
  
  
  
  config.vm.provision "shell", privileged: false, inline: <<-EOF
   cat /etc/hosts > /tmp/hosts
   echo "192.168.2.10 salt" >> /tmp/hosts
   sudo -u root cp /tmp/hosts /etc/hosts
  EOF
  
  config.vm.provision "shell", privileged: false, inline: <<-EOF
sudo yum -y install gcc-c++ perl patch libtool bison xz unzip compat-expat1 openssl098e
  EOF
  
  config.vm.provision "shell", privileged: false, inline: <<-EOF
sudo /usr/sbin/adduser platform
echo "dkqkffhs)(*" | sudo passwd platform --stdin
sudo /usr/sbin/adduser coupang
echo "gk!3dlsp@4zps" | sudo passwd coupang --stdin
  EOF
  
  config.vm.provision "shell", privileged: false, inline: <<-EOF
sudo awk '/# Cmnd_Alias DRIVERS = \\/sbin\\/modprobe/{p=1;print"# Cmnd_Alias DRIVERS = \\/sbin\\/modprobe\\nCmnd_Alias APACHE = \\/pang\\/program\\/apache-httpd\\/bin\\/apachectl\\nCmnd_Alias NGINX = \\/pang\\/program\\/nginx\\/sbin\\/nginx\\n\\n# Defaults specification";next}/# Defaults specification/{p=0;next}!p' /etc/sudoers > /tmp/sudoers.tmp
awk '/## Allow root to run any commands anywhere/{p=1;print"## Allow root to run any commands anywhere\\nroot    ALL=(ALL)       ALL\\nplatform        ALL=(ALL)       ALL\\ncoupang         ALL=(ALL)       NOPASSWD: APACHE, NGINX\\n\\n## Allows members of the \\x27""sys""\\x27 group to run networking, software,\\n## service management apps and more.";next}/## service management apps and more./{p=0;next}!p' /tmp/sudoers.tmp > /tmp/sudoers
chmod 440 /tmp/sudoers
sudo chown root.root /tmp/sudoers
sudo cp /tmp/sudoers /etc/sudoers
  EOF
  
  config.vm.provision "shell", privileged: false, inline: <<-EOF
sudo mkdir -p /pang/program
sudo mkdir -p /pang/service
sudo chown -R vagrant.vagrant /pang
  EOF


  config.vm.provision "shell", privileged: false, inline: <<-EOF
cd /tmp
rm -rf epel-release-6-8.noarch*
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
sudo rpm -ivh epel-release-6-8.noarch.rpm
  EOF
  
  
  config.vm.provision "shell", privileged: false, inline: <<-EOF
sudo /bin/sh -c "curl http://copr.fedoraproject.org/coprs/saltstack/zeromq4/repo/epel-6/saltstack-zeromq4-epel-6.repo > /etc/yum.repos.d/saltstack-zeromq4-epel-6.repo"
  EOF
  
  config.vm.provision "shell", privileged: false, inline: <<-EOF
sudo yum -y install python-devel swig openssl-devel zeromq
  EOF
  
  config.vm.provision "shell", privileged: false, inline: <<-EOF
cd /tmp
wget --no-check-certificate https://bootstrap.pypa.io/get-pip.py
sudo /usr/bin/python get-pip.py
  EOF
  
  config.vm.provision "shell", privileged: false, inline: <<-EOF
sudo pip install salt==2014.7.0
  EOF
  
  config.vm.provision "shell", privileged: false, inline: <<-EOF
sudo chown -R coupang.coupang /pang
EOF

end
