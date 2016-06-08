# clusterer
An ansible framework to encluster remote machines

## Installation

Ubuntu installation:
```sh
root@host:~# apt install ansible
```

ssh-copy-id

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) NOPASSWD:ALL

 sudo lxc-ls --fancy
 
sudo virsh -c lxc:/// domxml-from-native lxc-tools /var/lib/lxc/test-container-started/config

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

## Comments

I had to use a different vagrant image because the currently
maintained one has a `hostname` and `ifdown` and patch workarounds are
unwise. Better to wait for Vagrant 1.8.2 and any appropriate updates
to Ununtu 16.04. - SM

Had to downgrade to wily64 to get multiple network cards to work.

## TODO

* Address the above ifdown issue
* Use a current vagrant image rather than user image when above is fixes
* Create keys for inter box ssh

## License

GLPv3 - see LICENSE



add-apt-repository ppa:ubuntu-lxc/lxd-stable
apt update
apt dist-upgrade
apt install lxd

apt install zfsutils-linux

# lxd init
Name of the storage backend to use (dir or zfs): zfs
Create a new ZFS pool (yes/no)? yes
Name of the new ZFS pool: lxd
Would you like to use an existing block device (yes/no)? no
Size in GB of the new loop device (1GB minimum): 2
Would you like LXD to be available over the network (yes/no)? no
Do you want to configure the LXD bridge (yes/no)? yes
Warning: Stopping lxd.service, but it can still be activated by:
  lxd.socket
LXD has been successfully configured.


system@biscuit:~/clusterer$ sudo zpool list
NAME   SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP  HEALTH  ALTROOT
lxd   1.98G   448K  1.98G         -     0%     0%  1.00x  ONLINE  -

lxc remote add images images.linuxcontainers.org (may not be required)
lxc image list images:

lxc launch images:ubuntu/xenial/i386 ubuntu-test

system@biscuit:~/clusterer$ lxc launch images:ubuntu/xenial/i386 ubuntu-test
Creating ubuntu-test
Retrieving image: 100%
Starting ubuntu-test
system@biscuit:~/clusterer$ lxc list
+-------------+---------+-----------------------+----------------------------------------------+------------+-----------+
|    NAME     |  STATE  |         IPV4          |                     IPV6                     |    TYPE    | SNAPSHOTS |
+-------------+---------+-----------------------+----------------------------------------------+------------+-----------+
| ubuntu-test | RUNNING | 10.177.161.189 (eth0) | fd7f:fe66:b04:16c8:216:3eff:fef8:6884 (eth0) | PERSISTENT | 0         |
+-------------+---------+-----------------------+----------------------------------------------+------------+-----------+
system@biscuit:~/clusterer$ 
system@biscuit:~/clusterer$ lxc info ubuntu-test
#Name: ubuntu-test
Architecture: i686
Created: 2016/06/06 13:40 UTC
Status: Running
Type: persistent
Profiles: default
Pid: 21507
Ips:
  eth0:	inet	10.177.161.189	vethXMKUW7
  eth0:	inet6	fd7f:fe66:b04:16c8:216:3eff:fef8:6884	vethXMKUW7
  eth0:	inet6	fe80::216:3eff:fef8:6884	vethXMKUW7
  lo:	inet	127.0.0.1
  lo:	inet6	::1
Resources:
  Processes: 9
  Disk usage:
    root: 2.32MB
  Memory usage:
    Memory (current): 19.07MB
    Memory (peak): 21.32MB
  Network usage:
    eth0:
      Bytes received: 4.98kB
      Bytes sent: 1.44kB
      Packets received: 33
      Packets sent: 12
    lo:
      Bytes received: 0 bytes
      Bytes sent: 0 bytes
      Packets received: 0
      Packets sent: 0
system@biscuit:~/clusterer$ #


And you can also directly pull or push files from/to the container with:

lxc file pull ubuntu-test/path/to/file .
lxc file push /path/to/file ubuntu-test/
When done, you can stop or delete your container with one of those:

lxc stop ubuntu-test
lxc delete ubuntu-test

system@biscuit:~/clusterer$ lxc exec ubuntu-test /bin/bash
root@ubuntu-test:~# exit

make a container
alias it

http://www.redmine.org/projects/redmine/wiki/HowTo_Install_Redmine_on_Ubuntu_step_by_step

bind-address            = 0.0.0.0
./mysql.conf.d/mysqld.cnf:bind-address		= 127.0.0.1
service mysql restart

guest# mysql -u root -p mysql
Enter password:



grant all privileges on *.* to 'root'@'%' identified by 'PASSWORD' with grant option;

flush privileges;

host# sudo apt install mysql-client

sudo mysqldump -h 10.177.161.21 -p --all-databases > afile

system@biscuit:~$ lxc list
+----------------+---------+-----------------------+----------------------------------------------+------------+-----------+
|      NAME      |  STATE  |         IPV4          |                     IPV6                     |    TYPE    | SNAPSHOTS |
+----------------+---------+-----------------------+----------------------------------------------+------------+-----------+
| redmine-ubuntu | RUNNING | 10.177.161.158 (eth0) | fd7f:fe66:b04:16c8:216:3eff:feda:4166 (eth0) | PERSISTENT | 0         |
+----------------+---------+-----------------------+----------------------------------------------+------------+-----------+
system@biscuit:~$ dig redmine-ubuntu @10.177.161.1

; <<>> DiG 9.10.3-P4-Ubuntu <<>> redmine-ubuntu @10.177.161.1
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 26921
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1280
;; QUESTION SECTION:
;redmine-ubuntu.			IN	A

;; ANSWER SECTION:
redmine-ubuntu.		0	IN	A	10.177.161.158

;; Query time: 0 msec
;; SERVER: 10.177.161.1#53(10.177.161.1)
;; WHEN: Wed Jun 08 16:30:38 BST 2016
;; MSG SIZE  rcvd: 59

system@biscuit:~$ 

/etc/resolvconf/resolv.conf.d/head is a kludge