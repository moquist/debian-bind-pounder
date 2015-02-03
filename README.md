# debian-bind-pounder

This project stands up bind inside docker inside virtualbox via vagrant,
forwarding the following ports:

* localhost:60053/udp is forwarded to bind
* localhost:62375/tcp is forwarded to docker in the VM

## Dependencies

1. [vagrant](http://vagrantup.com)
1. [virtualbox](http://virtualbox.com)
1. [docker](http://docker.com)

## Configuration

* bind is configured by ```vmfiles/bind-conf/named.conf```
* bind is configured (by default) to serve only the records included in ```vmfiles/bind-conf/fw.zone``` and ```vmfiles/bind-conf/in-addr.arpa.zone```

## Usage

1. clone this repo
1. ```cd``` into the repo dir
1. ```vagrant up```
 1. This will take some time, more/less depending on your bandwidth and system. It takes about 90 seconds on my system.
1. When it's finished, ```dig @localhost -p60053 one-four```.
1. ```echo 'luggage-combo A 9.8.7.6' >> vmfiles/bind-conf/fw.zone```
1. ```echo '6.7.8.9 PTR luggage.combo.' >> vmfiles/bind-conf/in-addr.arpa.zone```
1. ```docker -H tcp://localhost:62375 kill -s HUP $(docker -H tcp://localhost:62375 ps -q)```
1. ```dig @localhost -p60053 luggage-combo```
1. ```dig @localhost -p60053 -x 9.8.7.6```



