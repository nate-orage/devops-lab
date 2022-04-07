# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/vagrant_data", disabled: false
  config.vm.provider "hyperv"

  config.vm.provider "hyperv" do |h|
    h.enable_virtualization_extensions = false
    h.linked_clone = false
    h.memory = 2048
  end
  config.vm.define 'ansible' do |ansible|
    ansible.vm.box = 'bento/ubuntu-18.04' 
    ansible.vm.hostname = 'ansible'
    ansible.vm.network 'public_network', bridge: "Realtek PCIe GBE Family Controller"
    # , ip: "192.168.1.206"
    ansible.vm.provision 'shell',
      inline: "sudo apt update && sudo apt upgrade -y && sudo apt install software-properties-common -y && sudo add-apt-repository --yes --update ppa:ansible/ansible && sudo apt install -t bionic-backports cockpit fail2ban ansible -y"
  end
  config.vm.define 'ub1' do |ub1|
    ub1.vm.box = 'bento/ubuntu-18.04'
    ub1.vm.hostname = 'ub1'
    ub1.vm.network 'public_network', bridge: "Realtek PCIe GBE Family Controller"
    # , ip: "192.168.1.206"
    ub1.vm.provision 'shell',
      inline: "sudo apt update && sudo apt upgrade -y && sudo apt install -t bionic-backports cockpit fail2ban -y"
  end
  config.vm.define 'cent1' do |cent1|
    cent1.vm.box = 'bento/centos-8'
    cent1.vm.hostname = 'cent1'
    cent1.vm.network 'public_network', bridge: "Realtek PCIe GBE Family Controller"
    # , ip: "192.168.1.206"
    cent1.vm.provision 'shell',
      inline: "sudo dnf --disablerepo '*' --enablerepo=extras -y swap centos-linux-repos centos-stream-repos && sudo dnf distro-sync -y && sudo yum update -y && sudo yum install epel-release cockpit -y && sudo yum install -y fail2ban && sudo systemctl start cockpit && sudo systemctl enable fail2ban"
  end
  # servers=[
    #     {
    #       :hostname => "ansible",
    #       :box => "bento/ubuntu-18.04",
    #     #   :ip => "172.16.1.50",
    #       :ssh_port => '2200',
    #       :provision => 'shell', inline: "sudo apt update && sudo apt upgrade -y && sudo apt install software-properties-common-y && sudo add-apt-repository --yes --update ppa:ansible/ansible && sudo apt install -t focal-backports cockpit fail2ban ansible -y"
    #     },
    #     {
    #       :hostname => "ub1",
    #       :box => "bento/ubuntu-18.04",
    #     #   :ip => "172.16.1.51",
    #       :ssh_port => '2201',
    #       :provision => 'shell', inline: "sudo apt update && sudo apt upgrade -y && sudo apt install -t focal-backports cockpit fail2ban -y"
    #     },
    #     {
    #       :hostname => "ub2",
    #       :box => "bento/ubuntu-18.04",
    #     #   :ip => "172.16.1.52",
    #       :ssh_port => '2202',
    #       :provision => 'shell', inline: "sudo apt update && sudo apt upgrade -y && sudo apt install -t focal-backports cockpit fail2ban -y"
    #     },
    #     {
    #         :hostname => 'cent1',
    #         :box => 'bento/centos-8',
    #         # :ip =>
    #         :ssh_port => '2203',
    #         :provision => 'shell', inline: "sudo yum update -y && sudo yum install epel-release cockpit fail2ban -y && sudo systemctl start cockpit && sudo systemctl enable fail2ban"
    #     }
    #   ]

    # vm.each do |machine|
    #     config.vm.define machine[:hostname] do |node|
    #         node.vm.box = machine[:box]
    #         node.vm.hostname = machine[:hostname]
    #         node.vm.network :public_network, bridge: "Realtek PCIe GBE Family Controller"
    #         # , ip: machine[:ip]
    #         node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"
    #         node.vm.provider :virtualbox do |vb|
    #             vb.customize ["modifyvm", :id, "--memory", 512]
    #             vb.customize ["modifyvm", :id, "--cpus", 1]
    #         end
    #     end
    # end
end