# Setup Enviroment
in this guide we will see how to run the enviroment (or how it was setup).<br>We strongly suggest to create a Virtual Machine with your favourite Linux distribution and following the next istructions.
1) [GNS3](setup-enviroment.md#GNS3 "GNS3") 
2) [ONOS](setup-enviroment.md#ONOS "ONOS") 
3) [OpenVSwitches](setup-enviroment.md#OVS "OVS") 
4) [FRR Routers](setup-enviroment.md#FRR "FRR Routers") 

**NOTE:** Only [GNS3](setup-enviroment.md#GNS3 "GNS3")  and [ONOS](setup-enviroment.md#ONOS "ONOS") instructions should be perform in order to run the project, others instructions are just the command uset to setup the enviroment but should be execute just once

## GNS3
in order to install GNS3 following the istructions guide made by GNS3:
- [Installation for Linux](https://docs.gns3.com/docs/getting-started/installation/linux/) (this will also install Docker)

#### After installation
Opening GNS3 and make sure that all paths under:
- Edit -> Preferences -> General (4 paths)
- Edit -> Preferences -> Binary Images (1 path)

Will point all to the respectly folders contained in the repository under `GNS3/*`

<details>
  <summary>Screenshots</summary>
  
  ![Paths of General Preferences](./images/paths-general.png "Paths of General Preferences")
  ![Paths of Binary Images Preferences](./images/paths-binary-images.png "Paths of Binary Images Preferences")
</details>

## ONOS
Since Docker should be alredy isntalled we can start to get an ONOS image and star deploying it:

Get ONOS image
```
docker pull onosproject/onos
```
Create ONOS Docker
```
docker run -t -d -p 8181:8181 -p 8101:8101 -p 5005:5005 -p 830:830 -p 6653:6653 --name onos onosproject/onos
```
If controller alredy present just start it
```
docker start onos 
```
once started check if everithing is working
- CLI:
  - `ssh -p 8101 -o StrictHostKeyChecking=no onos@localhost`
  - Pass: `rocks`
- test apps:
    - `apps -a -s`
- GUI via browser:
    - `http://localhost:8181/onos/ui` [(link)](http://localhost:8181/onos/ui)
- REST API
    - `http://localhost:8181/onos/v1/docs128` [(link)](http://localhost:8181/onos/v1/docs128)

### ONOS and GNS3
the presence of ONOS in the GNS3 project will be carry out with a cloud component, set up with a tap interface `host_tap`, devices can communicate to the controller throught that interface that must be configure from the host machine with those commands
```
ip addr add 192.168.100.10/24 dev host_tap
ip link set dev host_tap up
```

**NOTE:** From now on all istructions are unecessary in order to run the project

## OVS
In order to configure OpenVSwitches we assume that they will talk with the controller with interface `eth0`.<br>That interface will be manually set by OpenVSwitch network configuration file, which can be edit directly in GNS3 under `rught-click on the device -> Configure -> Network Configuration (last entry in the 'General Settings')`

To be sure that the switch will use `eth0` olny for communicate with the controller we must ensure this commands
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
