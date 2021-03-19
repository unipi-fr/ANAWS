# Project configuration
In this document we will keep track of all the steps to carry out the project.

**IMPORTANT:** In order to carry out by yourself those istructions you need first to [setup the enviroment](./setup-enviroment.md "ssetup-enviroment.md")

1) [OpenVSwitches](./project-configuration.md#OVS "OVS") 
2) [FRR Routers](./project-configuration.md#FRR "FRR Routers") 
   
## OVS
we assume that OpenVSwitches will talk with the controller with interface `eth0`.<br>The IP of that interface will be manually set by OpenVSwitch network configuration file, which can be edit directly in GNS3 under `rught-click on the device -> Configure -> Network Configuration (last entry in the 'General Settings')`

To be sure that the switch will use `eth0` olny for communicate with the controller we must ensure this commands on each OVS
```
ovs-vsctl del-port eth0
ovs-vsctl set-controller br0 tcp:192.168.100.10:6653
ovs-vsctl set controller br0 connection-mode=out-of-band
ovs-vsctl set controller br0 other-config:disable-in-band=true
```
## FRR
In the enviroment there will be 2 kinds of routers:
 1) **External Routers** that are routers on the borders of the AS (*Autonomous Systems*)
 2) **Internal Routers** that are the routers which comunicate with the ONOS controller with SDN-IP (In our case we will use just one *Internal Router*)
#### External Routers