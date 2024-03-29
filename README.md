# Ansible playbook for all-in-one FaxOCR
The playbook is designed to create an all-in-one FaxOCR on a variety of platforms. The FaxOCR consists of FaxOCR (Ruby on Rails), web server, mail server, and database server. The all-in-one FaxOCR contains all of them on a single platform.

## Target platform
Supported platforms where FaxOCR will be set up
- Bare-metal, virtual Machine
- LXD container
- Docker container

## Target type
- FaxOCR server
- [FaxOCR development environment with GUI](https://sites.google.com/site/faxocr2010/home/ji-pc-de-tamesu)

## OS of target node running FaxOCR
- Ubuntu 18.04 (amd64)
- Debian 10 (amd64)

## Control node
Tested environment of control node
- Ubuntu 18.04 (amd64)
- Ansible 2.9
- Docker CE 19.03.13
- LXD 3.0.3

## Installation
Clone this repository on the control node first.
```
git clone --recursive https://github.com/faxocr/faxocr-ansible
cd faxocr-ansible
```
If you want to change initial password for system accounts, mysql's accounts and etc, edit inventory/host_vars/secret.yml.

### Bare-metal, virtual machine
First, set up target machine
- Install Ubuntu 18.04 (amd64)
- Create an account (e.x. "deploy" in this document) for ansible
- Install OpenSSH server and place an authorized key of SSH for the deploy user in ~deploy/.ssh/authorized_keys
- Install sudo and set up sudoers without password or set login password of the deploy user
- Install python3

Next, set up some on the control node
```
echo 'IP address or hostname (resolvable) of target node' > inventory/standalone
```
Apply playbook (Choose one of the followings)
- FaxOCR server
```
ansible-playbook -u deploy -i inventory/standalone standalone.yml
```
- FaxOCR development environment with GUI
```
ansible-playbook -u deploy -i inventory/standalone gui.yml
```

### LXD container
First, create a LXD container named faxocr-all-in-one (e.x.) using Ubuntu 18.04 on the control node
```
faxocr_container_name=faxocr-all-in-one
lxc launch ubuntu/18.04 "$faxocr_container_name"
echo "$faxocr_container_name" > inventory/standalone
```
Apply playbook
```
ansible-playbook -c lxd -i inventory/standalone standalone.yml
```

### Docker container
Apply playbook
```
ansible-playbook -i inventory/docker docker.yml
```
After applying playbook, a docker image named faxocr-allinone will be created.

## LICENSE
MIT
