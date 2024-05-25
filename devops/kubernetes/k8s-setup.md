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
### Disable SELinux
```
sudo setenforce 0
sudo sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
```
### Disable Firewall Service
```
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```
### Configure ip_forward
```
sudo sysctl -w net.ipv4.ip_forward=1
```
### Add Node IP Addresses to /etc/hosts
```
sudo vim /etc/hosts
```
### Create SSH Key for Password-less SSH
Generate SSH key:
```
ssh-keygen
```
Copy SSH key to worker node:
```
ssh-copy-id worker1
```
Remove SSH key when IP configuration changes:
```
ssh-keygen -R <host>
```
### Change Hostname
```
sudo vim /etc/hostname
```
### Install vim and tree
```
sudo apt-get install vim tree
```
### Disable Swap
```
free -h
sudo swapoff -a
free -h
sudo vi /etc/fstab
```
Comment out all lines containing "swap" and save the file:
```
#/dev/mapper/centos-swap swap swap defaults 0 0
```
### Edit Sudoers File to Allow User to Run Commands Without Password
```
sudo vi /etc/sudoers
```
Add the following line at the end of the file and save:
```
sysadmin ALL=(ALL) NOPASSWD: ALL
```
### Install Docker (with ContainerD)
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo groupadd docker
sudo usermod -aG docker $USER
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```
### Install cri-docker Using .deb File
```
wget https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.13/cri-dockerd_0.3.13.3-0.ubuntu-focal_amd64.deb
sudo dpkg -i cri-dockerd_0.3.13.3-0.ubuntu-focal_amd64.deb
```
### Install kubeadm, kubelet, and kubectl
```
sudo apt-get update
sudo mkdir /etc/apt/keyrings
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
sudo systemctl enable --now kubelet
```
### Reset Kubernetes Cluster as need
```
sudo kubeadm reset --cri-socket=unix:///var/run/cri-dockerd.sock
sudo rm -rf $HOME/.kube/config
sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*
sudo apt autoremove
sudo rm -rf ~/.kube
sudo rm -rf /etc/cni /etc/kubernetes /etc/systemd/system/etcd*
sudo rm -rf /var/lib/dockershim /var/lib/etcd /var/lib/kubelet /var/lib/etcd2/ /var/run/kubernetes
docker image prune -a
docker volume prune -a
docker system prune -a
sudo systemctl restart docker
curl -sfL https://get.k3s.io | sh –/usr/local/bin/k3s-uninstall.sh
```
### Init Master Node
```
sudo systemctl restart kubelet
sudo kubeadm init --control-plane-endpoint=10.14.16.30 --pod-network-cidr=192.168.0.0/16 --node-name master1 --cri-socket=unix:///var/run/cri-dockerd.sock
```
### Configure kubectl for User
```
mkdir -p $HOME/.kube
sudo cp -f /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
### Create Token and Join Command on Master Node
If you forgot to save the join command when initializing the master node:
```
kubeadm token generate
```
Output: hytpjb.ma6xu508taxsk9yt
```
kubeadm token create hytpjb.ma6xu508taxsk9yt --print-join-command --ttl=0
```
### Join Worker Node
```
sudo kubeadm join 10.14.16.30:6443 --token hytpjb.ma6xu508taxsk9yt --discovery-token-ca-cert-hash sha256:8a2eb0a71c07b217c24eee3ccb6e0d829fde877ded493a9c9fb062d0b5a63caa --cri-socket=unix:///var/run/cri-dockerd.sock
```
### Allow Pods on Master Nodes

To allow scheduling of pods on the master node:
```
kubectl get nodes
kubectl taint nodes <node-name> node-role.kubernetes.io/ control-plane-
```
To apply to all master nodes:
```
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```
### Install Calico Network Plugin
```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/tigera-operator.yaml
curl https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/custom-resources.yaml -O
kubectl create -f custom-resources.yaml
watch kubectl get pods -n calico-system
```
### Install Rancher using Docker (Error: No supported version for kubernetes v.1.30.0)
```
docker run --name rancher-server -d --restart=always -p 6860:80 -p 6868:443 --privileged rancher/rancher:v2.8.3
docker logs rancher-server 2>&1 | grep "Bootstrap Password:"
```
After login, create new Custom Cluster, set Name, generate join command and join nodes
### Install Helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```
### Install Ingress Controller
```
helm upgrade --install ingress-nginx ingress-nginx
--repo https://kubernetes.github.io/ingress-nginx
--namespace ingress-nginx --create-namespace
```
### Install K9S
```
sudo apt update
sudo apt install snapd
sudo snap install k9s
sudo ln -s /snap/k9s/current/bin/k9s /snap/bin/
```
### Install Longhorn Storage
Install open-iscsi on all nodes:
```
sudo apt install open-iscsi
sudo apt install nfs-common
```
Configure helm values for Longhorn chart:
```
helm repo add longhorn https://charts.longhorn.io
helm repo update
helm search repo longhorn
helm pull longhorn/longhorn --version 1.6.1 --untar
cp longhorn/values.yaml xuandai-longhorn-values.yaml
sudo mkdir /longhorn-pv-data
vim xuandai-longhorn-values.yaml
```
Modify the `xuandai-longhorn-values.yaml` file with the following configuration:
```
service:
  ui:
    type: NodePort
    nodePort: 30888
  manager:
    type: ClusterIP
    
defaultDataPath: /longhorn-pv-data
replicaSoftAntiAffinity: true
storageMinimalAvailablePercentage: 15
upgradeChecker: false
defaultReplicaCount: 2
backupstorePollInterval: 500
nodeDownPodDeletionPolicy: do-nothing
guaranteedEngineManagerCPU: 15
guaranteedReplicaManagerCPU: 15

ingress:  
  enabled: true
  ingressClassName: longhorn-storage-ingress
  host: longhorn-ui.viettq.com

namespaceOverride: "storage"
```
Create the namespace and install Longhorn:
```
kubectl create ns storage
helm install longhorn-storage -f xuandai-longhorn-values.yaml longhorn --namespace storage
```
### Install Jenkins Server
Create a Docker network for Jenkins:
```
docker network create jenkins
```
Run Docker Dind container (to use docker inside docker container):
```
docker run –restart=always --name jenkins-docker --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind --storage-driver overlay2
```
Create a Dockerfile for Jenkins:
```
FROM jenkins/jenkins:2.440.3-jdk17
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins blueocean docker-workflow
```
Build the Jenkins image:
```
docker build -t myjenkins-blueocean:2.440.3-1 .
```
Run the Jenkins server:
```
docker run --name jenkins-server --restart=always --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.440.3-1
```
Retrieve the initial admin password:
```
docker logs jenkins-server
```
Install necessary plugins for Jenkins: Manage Jenkins -> Plugin -> Available plugins
+ Git
+ Docker Pipeline
Set up GitHub and DockerHub credentials: Go to global settings and add credentials with the following details:
+ Username: Your GitHub username
+ Password: Access token generated from GitHub
+ ID: Use a memorable ID like "github"
### Install ArgoCD Server
```
kubectl create ns argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/master/manifests/install.yaml
```
Retrieve the initial admin password (The default username for ArgoCD is "admin"):
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Set up ArgoCD repository connection via HTTPS:
+ Choose connection method: VIA HTTPS
+ Type: Git
+ Project: default
+ Repository URL: https://github.com/rockman88v/app-helmchart.git
+ Username/Password: GitHub account and token
Define application in ArgoCD:
+ Application Name: DEMO-APP3
+ Project Name: Default
+ Sync Policy: Manual or Auto-sync
+ Repository URL: https://github.com/rockman88v/app-helmchart.git
+ Path: app-demo
+ Cluster URL: https://kubernetes.default.svc
+ Namespace: demo3
+ Values Files: values.yaml and app-demo-value.yaml
### Install PGAdmin
```
helm repo add runix https://helm.runix.net
helm install pgadmin4 --set env.email=nguyenvoxuandai@thaco.com.vn --set env.password=thaco@1234 --set service.type=NodePort --set service.nodePort=31515 runix/pgadmin4 -n chat
```
### Install Postgres-HA
Install:
```
helm install postgres-ha --set postgresql.password=thaco@1234 --set postgresql.database=platform_db oci://registry-1.docker.io/bitnamicharts/postgresql-ha -n chat
```
Uninstall:
```
helm uninstall postgres-ha -n chat
```
### Install HAProxy
Add the required repository:
```
sudo add-apt-repository ppa/v8-devel
sudo apt-get update
sudo apt-get install rsyslog
```
Install HAProxy:
```
sudo apt install --no-install-recommends software-properties-common
sudo add-apt-repository ppa/haproxy-2.4 -y
sudo apt install haproxy=2.4.*
haproxy -v
```
Edit the HAProxy configuration file:
```
sudo vim /etc/haproxy/haproxy.cfg
```
Add the following configuration to the file:
```
global
    log         127.0.0.1:514 local0

    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     4000
    user        haproxy
    group       haproxy
    daemon
    stats socket /var/lib/haproxy/stats
    stats socket *:1999 level admin
    stats socket /var/run/haproxy.sock mode 600 level admin
    
defaults
    mode                    http
    log                     global
    option                  httplog
    option                  dontlognull
    option http-server-close
    option forwardfor       except 127.0.0.0/8
    option                  redispatch
    retries                 3
    timeout http-request    10s
    timeout queue           1m
    timeout connect         10s
    timeout client          1m
    timeout server          1m
    timeout http-keep-alive 10s
    timeout check           10s
    maxconn                 3000

listen stats
    bind *:8085
    stats enable
    stats uri /stats
    stats realm HAProxy-04\ Statistics
    stats auth admin:thaco@1234
    stats admin if TRUE

backend per_ip_and_url_rates
    stick-table type binary len 8 size 1m expire 24h store http_req_rate(24h)

backend per_ip_rates
    stick-table type ip size 1m expire 24h store gpc0,gpc0_rate(30s)

frontend frontend_ssl_443
        bind :80
        bind *:443 ssl crt /etc/haproxy/ssl/viettq_app.pem
        mode http
        option httpclose
        option forwardfor
        http-request add-header X-Forwarded-Proto https
        cookie  SRVNAME insert indirect nocache
        default_backend backend_ingress

        acl rancher hdr_dom(host) -i rancher.monitor.bangpdk.dev
        acl jenkins hdr_dom(host) -i jenkins.prod.bangpdk.dev
        acl argocd hdr_dom(host) -i argocd.prod.bangpdk.dev

        use_backend backend_rancher if rancher
        use_backend backend_jenkins if jenkins
        use_backend backend_argocd if argocd

backend backend_ingress
        mode    http
        stats   enable
        stats   auth admin:thaco@1234
        balance roundrobin
        server  worker1 10.14.16.36:30080 cookie p1 weight 1 check inter 2000
        server  worker2 10.14.16.37:30080 cookie p1 weight 1 check inter 2000

backend backend_rancher
        mode    http
        stats   enable
        stats   auth admin:thaco@1234
        balance roundrobin
        server  nfs 10.14.16.38:6860 cookie p1 weight 1 check inter 2000

backend backend_jenkins
        mode    http
        stats   enable
        stats   auth admin:thaco@1234
        balance roundrobin
        server  cicd 10.14.16.39:8080 cookie p1 weight 1 check inter 2000

backend backend_argocd
        mode    http
        stats   enable
        stats   auth admin:thaco@1234
        balance roundrobin
        server  worker2 10.14.16.37:32219 cookie p1 weight 1 check inter 2000
```
Validate the HAProxy configuration:
```
haproxy -c -f /etc/haproxy/haproxy.cfg
```
Restart and enable HAProxy service:
```
sudo service haproxy restart
sudo systemctl enable haproxy
```
---
**Note:** ArgoCD does not allow access via self-signed certificates, so additional configuration may be required for domain access.



## ========> Setup on Centos Server <========

Cluster with 3 Control Plane Nodes, 2 Worker Nodes, 1 NFS Server and 1 CICD Server
