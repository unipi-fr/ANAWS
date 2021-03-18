ssh -p 8101 -o StrictHostKeyChecking=no onos@localhost

ifconfig host_tap 192.168.100.10 netmask 255.255.255.0 up

ovs-vsctl set-controller br0 tcp:192.168.100.10:6653

ovs-vsctl set controller br0 connection-mode=out-of-band

ovs-vsctl set controller br0 other-config:disable-in-band=true

ovs-vsctl del-port eth0