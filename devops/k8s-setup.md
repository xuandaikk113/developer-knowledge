# Kubernetes Cluster Setup (On Premise)
This is the instruction with step by step to set up a kubernetes cluster onpremise.

## ========> Setup on Ubuntu Server <========
Cluster with 1 Control Plane Node and 1 Worker Node
### Change the IP Address of the Server to Manual
#### Using GUI
Command to switch from CLI to GUI:
```
systemctl isolate graphical.target
```
Go to Wired Setting > IPv4 > Manual > 10.14.16.30 > 255.255.254.0 > 10.14.17.1 > DNS 8.8.8.8,8.8.4.4

#### Using CLI
Command to switch from GUI to CLI:
```
systemctl isolate multi-user.target
```
#### Install Open-SSH
Command to switch from GUI to CLI:
```
sudo apt install openssh-server
```




## ========> Setup on Centos Server <========
Cluster with 3 Control Plane Nodes, 2 Worker Nodes, 1 NFS Server and 1 CICD Server
