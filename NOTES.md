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


----------

apt-add-repository ppa:ansible/ansible
apt update
apt install ansible
apt install debconf-utils


lxd	lxd/bridge-dnsmasq	string	
lxd	lxd/bridge-domain	string	lxd
lxd	lxd/bridge-empty-error	note	
lxd	lxd/bridge-http-proxy	boolean	false
lxd	lxd/bridge-ipv4-address	string	10.41.63.1
lxd	lxd/bridge-ipv4	boolean	true
lxd	lxd/bridge-ipv4-dhcp-first	string	10.41.63.2
lxd	lxd/bridge-ipv4-dhcp-last	string	10.41.63.254
lxd	lxd/bridge-ipv4-dhcp-leases	string	252
lxd	lxd/bridge-ipv4-nat	boolean	true
lxd	lxd/bridge-ipv4-netmask	string	24
lxd	lxd/bridge-ipv6-address	string	fd6d:c772:b128:ebfd::1
lxd	lxd/bridge-ipv6	boolean	true
lxd	lxd/bridge-ipv6-nat	boolean	true
lxd	lxd/bridge-ipv6-netmask	string	64
lxd	lxd/bridge-name	string	lxdbr0
lxd	lxd/bridge-random-warning	note	
lxd	lxd/bridge-upgrade-warning	note	
lxd	lxd/setup-bridge	boolean	true
lxd	lxd/update-profile	boolean	true
lxd	lxd/use-existing-bridge	boolean	false

root@busy:~# diff a b
6,11c6,11
< lxd	lxd/bridge-http-proxy	boolean	true
< lxd	lxd/bridge-ipv4-address	string	
< lxd	lxd/bridge-ipv4	boolean	false
< lxd	lxd/bridge-ipv4-dhcp-first	string	
< lxd	lxd/bridge-ipv4-dhcp-last	string	
< lxd	lxd/bridge-ipv4-dhcp-leases	string	
---
> lxd	lxd/bridge-http-proxy	boolean	false
> lxd	lxd/bridge-ipv4-address	string	10.41.63.1
> lxd	lxd/bridge-ipv4	boolean	true
> lxd	lxd/bridge-ipv4-dhcp-first	string	10.41.63.2
> lxd	lxd/bridge-ipv4-dhcp-last	string	10.41.63.254
> lxd	lxd/bridge-ipv4-dhcp-leases	string	252
13,17c13,17
< lxd	lxd/bridge-ipv4-netmask	string	
< lxd	lxd/bridge-ipv6-address	string	
< lxd	lxd/bridge-ipv6	boolean	false
< lxd	lxd/bridge-ipv6-nat	boolean	false
< lxd	lxd/bridge-ipv6-netmask	string	
---
> lxd	lxd/bridge-ipv4-netmask	string	24
> lxd	lxd/bridge-ipv6-address	string	fd6d:c772:b128:ebfd::1
> lxd	lxd/bridge-ipv6	boolean	true
> lxd	lxd/bridge-ipv6-nat	boolean	true
> lxd	lxd/bridge-ipv6-netmask	string	64

ansible localhost -m service -a "name=networking state=restarted"

lxc launch ubuntu:16.04 first

root@busy:~# lxc launch ubuntu:16.04 first
Generating a client certificate. This may take a minute...
If this is your first time using LXD, you should also run: sudo lxd init

Creating first
Retrieving image: 100%
Starting first
root@busy:~# lxc list
+-------+---------+--------------------+-----------------------------------------------+------------+-----------+
| NAME  |  STATE  |        IPV4        |                     IPV6                      |    TYPE    | SNAPSHOTS |
+-------+---------+--------------------+-----------------------------------------------+------------+-----------+
| first | RUNNING | 10.41.63.27 (eth0) | fd6d:c772:b128:ebfd:216:3eff:fea7:69c6 (eth0) | PERSISTENT | 0         |
+-------+---------+--------------------+-----------------------------------------------+------------+-----------+
root@busy:~# ifconfig
eno1      Link encap:Ethernet  HWaddr b8:ae:ed:ea:69:41  
          inet addr:192.168.0.3  Bcast:192.168.0.255  Mask:255.255.255.0
          inet6 addr: fe80::baae:edff:feea:6941/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:111698 errors:0 dropped:3 overruns:0 frame:0
          TX packets:57636 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:160405691 (160.4 MB)  TX bytes:4315216 (4.3 MB)
          Interrupt:16 Memory:df100000-df120000 

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:163 errors:0 dropped:0 overruns:0 frame:0
          TX packets:163 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:12576 (12.5 KB)  TX bytes:12576 (12.5 KB)

lxdbr0    Link encap:Ethernet  HWaddr fe:5c:57:cd:bd:72  
          inet addr:10.41.63.1  Bcast:0.0.0.0  Mask:255.255.255.0
          inet6 addr: fd6d:c772:b128:ebfd::1/64 Scope:Global
          inet6 addr: fe80::d08c:f9ff:fed9:5cd1/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:24 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:1308 (1.3 KB)  TX bytes:3120 (3.1 KB)

vethD7L5U9 Link encap:Ethernet  HWaddr fe:5c:57:cd:bd:72  
          inet6 addr: fe80::fc5c:57ff:fecd:bd72/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:21 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:1476 (1.4 KB)  TX bytes:2314 (2.3 KB)

 ansible localhost -m debconf -a "name=lxd question='lxd/update-profile' value=true vtype='boolean'"


ansible localhost -m apt -a "name=lxd state=present"
ansible localhost -m service -a "name=networking state=restarted"
ansible localhost -m debconf -a "name=lxd question='lxd/dupdate-profile' value=false vtype='boolean'"
ansible localhost -m debconf -a "name=lxd question='lxd/update-profile' value=false vtype='boolean'"
ansible localhost -m debconf -a "name=lxd question='lxd/update-profile' value=true vtype='boolean'"

--- resolveconf

echo "nameserver 10.177.161.1" | resolvconf -a lxdbr0.net

(does not survive reboot)

--- ha proxy

do all this remotely

apt install haproxy (no interaction required)

deploy@target:~$ netstat --tcp --listen
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 *:ssh                   *:*                     LISTEN     
tcp6       0      0 [::]:ssh                [::]:*                  LISTEN     
deploy@target:~$ 

global
	log /dev/log	local0
	log /dev/log	local1 notice
	chroot /var/lib/haproxy
	stats socket /run/haproxy/admin.sock mode 660 level admin
	stats timeout 30s
	user haproxy
	group haproxy
	daemon

	# Default SSL material locations
	ca-base /etc/ssl/certs
	crt-base /etc/ssl/private

	# Default ciphers to use on SSL-enabled listening sockets.
	# For more information, see ciphers(1SSL). This list is from:
	#  https://hynek.me/articles/hardening-your-web-servers-ssl-ciphers/
	ssl-default-bind-ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS
	ssl-default-bind-options no-sslv3

defaults
	log	global
	mode	http
	option	httplog
	option	dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
	errorfile 400 /etc/haproxy/errors/400.http
	errorfile 403 /etc/haproxy/errors/403.http
	errorfile 408 /etc/haproxy/errors/408.http
	errorfile 500 /etc/haproxy/errors/500.http
	errorfile 502 /etc/haproxy/errors/502.http
	errorfile 503 /etc/haproxy/errors/503.http
	errorfile 504 /etc/haproxy/errors/504.http

frontend http-in
    bind *:8080
    acl is_here hdr_beg(host) -i here.
    acl is_here_1 hdr_beg(host) -i here1.
    
    use_backend here if is_here
    use_backend here_1 if is_here_1
    default_backend here

backend here
    balance roundrobin
    cookie SERVERID insert nocache indirect
    option httpchk HEAD /check.txt HTTP/1.0
    option httpclose
    option forwardfor
    server here here.local:80 cookie Server1

backend here_1
    balance roundrobin
    cookie SERVERID insert nocache indirect
    option httpchk HEAD /check.txt HTTP/1.0
    option httpclose
    option forwardfor
    server lxd1 redmine-ubuntu:80 cookie Container1

