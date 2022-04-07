# Devops-lab

This repo is an ongoing lab meant to display different automated LAN deployments. The Vagrantfile will prompt Virtualbox to spawn one or many Ubuntu (ub) and CentOs (cent) servers. After the VMs are created, ansible playbooks are used to manage the servers. I have 1 other LAN server and 1 cloud server that I manage as well. I won't include those below. Other configuration files will be added over time.

## Using Vagrant

To learn how to use Vagrant, [search Youtube for tutorials](https://www.youtube.com/results?search_query=vagrant+101).

I use Vagrant on Windows with Virtualbox. Open Powershell in this project's root and type:

```powershell
vagrant up
```

Vagrant will read "./Vagrantfile",  fetch the Vagrant boxes and provision the machines below with software and SSH keys.

## Using Ansible

Ansible playbooks are stored in "./ansible". You can run a playbook by typing:

```bash:
cd ansible/
ansible-playbook -i hosts.yaml ubuntu-upgrade.yaml -K
```

## Default Provision

- Hosts: The below VMs are created and ansible's public key is copied to ub1 and cent1
  - ansible:
    - https://app.vagrantup.com/bento/boxes/ubuntu-18.04
    - [Ansible](https://www.ansible.com/)
    - [Cockpit](https://cockpit-project.org/)
    - [fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page)
  - ub1:
    - https://app.vagrantup.com/bento/boxes/ubuntu-18.04
    - [Cockpit](https://cockpit-project.org/)
    - [fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page)
  - cent1
    - https://app.vagrantup.com/bento/boxes/centos-8
    - [fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page)
- Services
  - Vagrant: VM provisioning
  - VirtualBox: VM creation
  - Ansible: Automation