# clusterer
An ansible framework to encluster remote machines

## Server Configuration

Vanilla Ubuntu 16.04 Server

ifdown eno1

auto eno1
iface eno1 inet static
	address 192.168.0.3
	netmask 255.255.255.0
	gateway 192.168.0.1
	dns-nameservers 192.168.0.1

ifup eno1

apt install openssh-server openssh-client

## Client Installation

user@master$ cd .../clusterer

[servers]
target1
target2

[servers:vars]
ansible_connection=ssh
ansible_user=system

copy your own or gen

user@master$ ssh-keygen -t rsa -f ansible_rsa  -q -N ""

root@master$ apt install software-properties-common
root@master$ apt-add-repository ppa:ansible/ansible
root@master$ apt update && apt upgrade
root@master$ apt install ansible

update "servers" in inventory

user@master$ ansible-playbook -i <inventory> --user system --ask-pass --ask-become-pass playbooks/init.yml

user@master$ ansible-playbook -i <inventory> playbooks/site.yml

## Contributing

1. Fork the project repo
2. Create your feature branch: `git checkout -b myfeature`
3. Commit your changes: `git commit -am 'Added my feature'`
4. Push to the branch: `git push origin myfeature`
5. Submit a pull request

## Credits

Steve Marvell (SM) - Systems Architect - Unipart Digital

## History

TODO

