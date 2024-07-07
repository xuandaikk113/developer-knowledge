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
### Install Open-SSH
```
sudo apt install openssh-server
```
### Disable Firewall
```
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```
### Configure IP Forward
```
sudo sysctl -w net.ipv4.ip_forward=1
```
### Configure Hosts file
```
sudo vim /etc/hosts
```
### Configure Hostname
```
sudo vim /etc/hostname
sudo reboot
```
### Generate SSH key for faster SSH
```
ssh-keygen
```
Press Enter multiple times after ssh-keygen command
```
ssh-copy-id worker1
```
In case want to remove old key, use command below:
```
ssh-keygen -R <host>
```
### Install necessary packages
```
sudo apt install vim tree curl wget
```

























## ========> Setup on Centos Server <========
Cluster with 3 Control Plane Nodes, 2 Worker Nodes, 1 NFS Server and 1 CICD Server
