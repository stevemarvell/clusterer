Note the consistent inventory names irrespective of actual hostname

server01  ansible_ssh_host=10.1.2.21
server02  ansible_ssh_host=10.1.2.22
server03  ansible_ssh_host=10.1.2.23

[servers]
server[01:03]

[servers:vars]
ansible_connection=ssh
ansible_user=system
public_iface=enp0s3
management_iface=enp0s9

[servers:vars]
ansible_connection=ssh
ansible_user=system
public_iface=enp0s3
management_iface=enp0s9

[galera_cluster]
server01
server02
server03

[rabbitmq_cluster]
server02
server03
server01

[postgresql_cluster]
server03
Server02

[maas_region]
server03

[maas_control]
server02
server03

[debugs:children]
servers