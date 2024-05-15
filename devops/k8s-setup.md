# Kubernetes Cluster Setup (Onpremise)
This is the instruction with step by step to set up a kubernetes cluster onpremise.

## Setup on Ubuntu Server
Cluster with 1 Control Plane Node and 1 Worker Node
### Change the IP Address of the Server to Manual
#### Using GUI
Command to switch from CLI to GUI:
```
systemctl isolate graphical.target
```
#### Using CLI
Command to switch from GUI to CLI:
```
systemctl isolate multi-user.target
```




## Setup on Centos Server
Cluster with 3 Control Plane Nodes, 2 Worker Nodes, 1 NFS Server and 1 CICD Server
