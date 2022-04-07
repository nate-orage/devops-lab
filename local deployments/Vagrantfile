# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  #SSH Keys for Ansible
  config.vm.synced_folder './keys', '/home/vagrant/keys'
  config.vm.provider "hyperv"
  config.vm.boot_timeout = 600
  config.vm.network 'public_network', bridge: "Realtek PCIe GBE Family Controller"
  config.vm.provider "hyperv" do |h|
    h.enable_virtualization_extensions = false
    h.linked_clone = false
    h.memory = 2048
  end
  config.vm.define 'ansible' do |ansible|
    #Bento ubuntu machines are used.
    ansible.vm.box = 'bento/ubuntu-18.04' 
    ansible.vm.hostname = 'ansible'
    ansible.vm.network 'public_network', bridge: "Realtek PCIe GBE Family Controller"
    #Ansible folder for scripts
    ansible.vm.synced_folder './ansible', '/home/vagrant/ansible' 
    #Provision ansible box with ansible, cockpit, and fail2ban
    ansible.vm.provision 'shell',
      inline: "cp /home/vagrant/.ssh/id_rsa.pub /home/vagrant/keys/ansible.pub && sudo systemctl restart ssh.service && sudo apt update && sudo apt upgrade -y && sudo apt install software-properties-common -y && sudo add-apt-repository --yes --update ppa:ansible/ansible && sudo apt install -t bionic-backports cockpit fail2ban ansible -y"
  end
  config.vm.define 'ub1' do |ub1|
    ub1.vm.box = 'bento/ubuntu-18.04'
    ub1.vm.hostname = 'ub1'
    #ub1 folder for scripts
    ub1.vm.synced_folder './ub1', '/home/vagrant/ub1'  
    #Provision ub1 box with cockpit and fail2ban
    ub1.vm.provision 'shell',
      inline: "sudo apt update && sudo apt upgrade -y && sudo apt install -t bionic-backports cockpit fail2ban -y && cat /home/vagrant/keys/ansible.pub >> /home/vagrant/.ssh/authorized_keys "
  end
  config.vm.define 'cent1' do |cent1|
    cent1.vm.box = 'bento/centos-8'
    cent1.vm.hostname = 'cent1'
    #Any cent1 files
    cent1.vm.synced_folder './cent1', '/home/vagrant/cent1'
    #Provision cent1 box with cockpit and fail2ban. Also start cockpit and enable fail2ban.
    cent1.vm.provision 'shell',
      inline: "sudo dnf --disablerepo '*' --enablerepo=extras -y swap centos-linux-repos centos-stream-repos && sudo dnf distro-sync -y && sudo yum update -y && sudo yum install epel-release cockpit -y && sudo yum install -y fail2ban && sudo systemctl start cockpit && sudo systemctl enable fail2ban && cat /home/vagrant/keys/ansible.pub >> /home/vagrant/.ssh/authorized_keys && chown vagrant:vagrant /home/vagrant/.ssh/* && sudo service sshd restart"  
  end
end