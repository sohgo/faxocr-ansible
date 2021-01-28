# Ansible playbook for all-in-one FaxOCR
The playbook is designed to create an all-in-one FaxOCR on a variety of platforms. The FaxOCR consists of FaxOCR (Ruby on Rails), web server, mail server, and database server. The all-in-one FaxOCR contains all of them on a single platform.

## Target platforms
Supported platforms where FaxOCR will be set up
- Bare-metal, Virtual Machine
- LXD Container
- Docker Container

## Target types
- FaxOCR server
- FaxOCR development environment with GUI

## OS of target node running FaxOCR
- Ubuntu 18.04 (amd64)

## Control node
A tested environment of control node
- Ubuntu 18.04 (amd64)
- Ansible 2.9
- Docker CE 19.03.13
- LXD 3.0.3

## Installation
### Bare-metal, Virtual machine
First, set up target machine
- Install Ubuntu 18.04
- Create an account (e.x. deploy) for ansible
- Place an authorized key of SSH for the deploy user in ~deploy/.ssh/authorized_keys
- Install sudo and set up sudoers without password or set login password for the deploy user

Next, set up some on the control node
```		
echo 'IP address or hostname (resolvable) of target node' > inventory/standalone
```		
Apply playbook (Choose one of the followings)
- FaxOCR server
```
ansible-play -u deploy -i inventory/standalone standalone.yml
```
- FaxOCR development environment with GUI
```
ansible-play -u deploy -i inventory/standalone gui.yml
```

### LXD Container
First, create a LXD container named faxocr-all-in-one (e.x.) using Ubuntu 18.04 on the control node
```
faxocr_container_name=faxocr-all-in-one
lxc launch ubuntu/18.04 "$faxocr_container_name"
echo "$faxocr_container_name" > inventory/standalone
```
Apply playbook
```
ansible-play -c lxd -i inventory/standalone standalone.yml
```

### Docker Container
Apply playbook
```
ansible-play -i inventory/standalone docker.yml
```
After applying playbook, a docker image named faxocr-allinone will be created.

## LICENSE
MIT
