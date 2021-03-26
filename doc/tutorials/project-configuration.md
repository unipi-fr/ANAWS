# Project configuration
In this document we will keep track of all the steps to carry out the project.

**IMPORTANT:** In order to carry out by yourself those istructions you need first to [setup the enviroment](./setup-enviroment.md "ssetup-enviroment.md")

1) [OpenVSwitches](./project-configuration.md#OVS "OVS") 
2) [FRR Routers](./project-configuration.md#FRR "FRR Routers") 
   
## OVS-Switch setup
we assume that OpenVSwitches will talk with the controller with interface `eth0`.<br>The IP of that interface will be manually set by OpenVSwitch network configuration file, which can be edit directly in GNS3 by doing `right-click on the specific ovs-device -> Configure -> Network Configuration (last entry in the 'General Settings')`
```
auto eth0
iface eth0 inet static
	address 192.168.100.X
	netmask 255.255.255.0
```

To be sure that the switch will use `eth0` only to communicate with the controller, and not to exange data with other ovs-switch, we must issue this commands on each OVS, (the last 2 command are not necessary)
```
ovs-vsctl del-port eth0
ovs-vsctl set-controller br0 tcp:192.168.100.10:6653
```
```
ovs-vsctl set controller br0 connection-mode=out-of-band
ovs-vsctl set controller br0 other-config:disable-in-band=true
```
## FRR Router setup
In the enviroment there will be 2 kinds of routers:
 1) **External Routers** that are routers on the borders of the AS (*Autonomous Systems*)
 2) **Internal Routers** that are the routers which comunicate with the ONOS controller with SDN-IP (In our case we will use just one *Internal Router*)
#### External Routers

<details>
  <summary>Network Details</summary>

  #### AS1
  - Network `192.168.1.0/24`
    - R1 `192.168.1.1` on interface `eth7`
    - R2 `192.168.1.2` on interface `eth7`
    - H11 `192.168.1.11` on interface `e0`
  #### AS2
  - Network `192.168.2.0/24`
    - R3 `192.168.2.3` on interface `eth7`
    - R4 `192.168.2.4` on interface `eth7`
    - H21 `192.168.2.21` on interface `e0`
  #### AS3
  - Network `192.168.3.0/24`
    - R5 `192.168.3.5` on interface `eth7`
    - H31 `192.168.3.31` on interface `e0`
  #### BGP sessions
  - R1 `eth0` `10.255.1.1/30` &#10231; Internal `eth7` `10.255.1.2/30`
  - R2 `eth0` `10.255.2.1/30` &#10231; Internal `eth7` `10.255.2.2/30`
  - R3 `eth0` `10.255.3.1/30` &#10231; Internal `eth7` `10.255.3.2/30`
  - R4 `eth0` `10.255.4.1/30` &#10231; Internal `eth7` `10.255.4.2/30`
  - R5 `eth0` `10.255.5.1/30` &#10231; Internal `eth7` `10.255.5.2/30`
</details>

To configure an ip address `A.B.C.D/M` on a given interface `<eth*>` issue the following command from the configuration mode
```
interface <eth*>
ip address A.B.C.D/M
```
activate BGP on the router from the configuration mode
```
router bgp <ASN>
```
give an ID to the router for the BGP sessions
```
bgp router-id A.B.C.D
```
announce a network `A.B.C.D/M`
```
network A.B.C.D/M
```
add a neighbor with ip `A.B.C.D` and AS number `X`
```
neighbor A.B.C.D remote-as X
```
<details>
  <summary>Chosen configuration</summary>
  
  - R1 `1.1.1.1` with ASN = `1`
  - R2 `2.2.2.2` with ASN = `1`
  - R3 `3.3.3.3` with ASN = `2`
  - R4 `4.4.4.4` with ASN = `2`
  - R5 `5.5.5.5` with ASN = `3`
  - internal `7.7.7.7` with ASN = `7`
</details>


#### Internal Router
As said before we will have only internal router
<details>
  <summary>Configuration</summary>

  #### IPs
  - on interface `eth7`
    - `10.255.1.2/30`
    - `10.255.2.2/30`
    - `10.255.3.2/30`
    - `10.255.4.2/30`
    - `10.255.5.2/30`
  - on interface `eth0`
    - `192.168.100.11/24`

</details>


## VIrtual Hosts
To configure ip on virtual host given ip address `A.B.C.D/M` and default gateway ip = `X.Y.W.Z` , open a console and issue
```
ip A.B.C.D/M X.Y.W.Z
```
