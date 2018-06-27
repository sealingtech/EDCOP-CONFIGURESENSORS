# EDCOP Configure Sensors

Table of Contents
-----------------
 
* [Configuration Guide](#configuration-guide)
* [Image Repository](#image-repository)
* [Cluster Settings](#cluster-settings)
* [Network Interface](#network-interfaces)
* [Node Selector](#nodeselector)

# Configuration Guide

Within this configuration guide, you will find instructions for modifying Host-Setup.  Host-Setup is designed to help deploy SR-IOV networks on physical clusters and MACVLAN networks on virtual clusters, while also optimizing performance across all sensors.

WARNING: While Host-setup doesn't require a reboot, there should be no sensors deployed before running this tool.  Delete all sensors utilizing either of the inline or passive networks.  Once this has been run it is possible to re-run this.
 
## Image Repository

This points to the location of the Docker container registry.  If you're changing these values, make sure you include the full repository name.
 
```
images:
    host-setup: gcr.io/edcop-public/host-setup:2
```

## Cluster Settings

Currently, the host-setup can be applied to physical or virtual clusters by using the MACVLAN plugin instead of the SR-IOV plugin for virtual clusters. The cluster type should be set to ```physical``` or ```virtual``` depending on your environment. Additionally, if you do not wish to use inline capabilities, you can disable the inline NIC setup by setting ```inline``` to ```false```. 

```
cluster:
  type: physical
  inline: true
```

## Network Interfaces

Configure specific settings to define the network interface cards.  The deviceName settings must the be the same across the cluster.  The numOfVirtualFunctions setting will be the number of virtual functions that will be created on each physicalNIC.  Each physical sensor will consume one SR-IOV virtual function. Virtual deployments do not use the MACVLAN plugin instead of SR-IOV, meaning the numOfVirtualFunctions settings are ignored.  Finally, the irqCoreAssignment is the CPU core that will handle all interrupts for the Network Interface Card.

```
networkInterfaces: 
    inline1interface:
        #Configure the interface names
        deviceName: enp216s0f1
        #Configure the number of virtual functions to be created on the interface
        numOfVirtualFunctions: 4
        #Configure the core that will handle interrupts for the network interface
        irqCoreAssignment: 8
    inline2interface:
        deviceName: enp216s0f2
        numOfVirtualFunctions: 4
        irqCoreAssignment: 8
    passive1interface:
        deviceName: enp216s0f3
        numOfVirtualFunctions: 4
        irqCoreAssignment: 8    
```  

## NodeSelector
The Nodetype option will configure which systems Configure Sensors will be applied to.  This should only be systems which will carry sensors and should NEVER include the master node.
  
```
nodeSelector:
  nodetype: worker
```
