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

<details>
  <summary>Networks involved</summary>
  
  #### AS1 has `192.168.1.0/24`
  - R1 `192.168.1.1` on interface `eth7`
  - R2 `192.168.1.2` on interface `eth7`
  - H11 `192.168.1.11` on interface `e0`
  #### AS2 has `192.168.2.0/24`
  - R3 `192.168.2.3` on interface `eth7`
  - R4 `192.168.2.4` on interface `eth7`
  - H21 `192.168.2.21` on interface `e0`
  #### AS3 has `192.168.3.0/24`
  - R5 `192.168.3.5` on interface `eth7`
  - H31 `192.168.3.31` on interface `e0`
  #### Routers
  - R1 `10.255.1.1/30` on interface `eth0`
  - R2 `10.255.2.1/30` on interface `eth0`
  - R3 `10.255.3.1/30` on interface `eth0`
  - R4 `10.255.4.1/30` on interface `eth0`
  - R5 `10.255.5.1/30` on interface `eth0`
</details>

configure ip address `A.B.C.D/M` on a given interface `<eth*>` (from configuration mode)
```
interface <eth*>
ip address A.B.C.D/M
```
activate BGP on the router issuing
```
router bgp <ASN>
```
give an ID to the router for the BGP sessions
```
bgp router-id A.B.C.D
```
<details>
  <summary>Chosen configuration</summary>
  
  - R1 `10.0.1.1` with ASN = `1`
  - R2 `10.0.2.1` with ASN = `1`
  - R3 `10.0.3.1` with ASN = `2`
  - R4 `10.0.4.1` with ASN = `2`
  - R5 `10.0.5.1` with ASN = `3`
</details>

## VIrtual Hosts
To configure ip on virtual host given ip address `A.B.C.D/M` and default gateway ip = `X.Y.W.Z` , open a console and issue
```
ip A.B.C.D/M X.Y.W.Z
```
