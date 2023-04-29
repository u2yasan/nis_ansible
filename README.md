# Ansible for NEM Supernode Program#

Ansible 
# This will attempt to install NEM NIS .
# No guarantees that it will even work. Use at your own risk.
# NEVER EVER expose your private keys on a VPS
# Use a VPS server for secure nem only

## What's inside ##

* Tested on
* VPS Contabo Ubuntu 22.04 64-bit 
* macOS

## Prerequisites ##
1. Linux or Mac or Windows can run ansible 2.0 >

## Setup steps ##

confirm root login from your localmachine
## macOS
% ssh root@<VPS IP address>
The authenticity of host '<VPS IP address> (<VPS IP address>)' can't be established.
ED25519 key fingerprint is SHA256:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '<VPS IP address>' (ED25519) to the list of known hosts.
root@<VPS IP address>'s password:
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.0-25-generic x86_64)
# exit

## macOS
## install brew https://brew.sh/
% /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
## install ansible and roles
% brew install ansible
% brew install esolitos/ipa/sshpass

% ansible-galaxy collection install community.general
% ansible-galaxy install geerlingguy.swap
% ansible-galaxy install lean_delivery.java

% ssh root@VPSServerIP

% vi hosts
[servers]
server1 ansible_host=<VPS IP address>

% ansible-inventory --list -y -i hosts
all:
  children:
    servers:
      hosts:
        server1:
          ansible_host: <VPS IP address>
    ungrouped: {}

% ansible all -m ping -u root -i hosts --ask-pass
SSH password:
server1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}

% vi host_vars/vault.yml
vault_admin_user_password: admin_password
vault_nis_boot_key: delegate_private_key

% ansible-vault encrypt \
    --vault-id nem@prompt \
    host_vars/vault.yml 

% ansible-vault decrypt \
    --vault-id nem@prompt \
    host_vars/vault.yml 

% ansible-playbook -i hosts playbook.yml -u root --ask-pass --ask-vault-pass

% ssh nem@


http://<VPS IP address>:7890/node/extended-info
http://<VPS IP address>:7880/nr/metaData
