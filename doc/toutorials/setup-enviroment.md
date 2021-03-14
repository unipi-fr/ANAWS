# Setup Enviroment
in this guide we will see how to setup the enviroment.

We strongly suggest to create a Virtual Machine with your favourite Linux distribution and following the next istructions.

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
    - `http://localhost:8181/onos/ui`
- REST API
    - `http://localhost:8181/onos/v1/docs128`
