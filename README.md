# SingleDockerRancher
How to install and configure a single Rancher server node with Docker in Ubuntu

## Requirements
- Linux Server (in this scenario I use Ubuntu in Azure)
- Docker application installed via Aptitude
- Rancher Server container
- A cloud or vSphere environment for K8s deployement and management (I use Azure)

## Resources
Install Ubuntu server on Azure
```
https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal
```

Install Docker on Ubuntu
```
https://docs.docker.com/engine/install/ubuntu/
```

Deploy Rancher as a container
```
https://rancher.com/docs/rancher/v2.5/en/installation/other-installation-methods/single-node-docker/
```

Create an AKS cluster via Rancher
```
https://rancher.com/docs/rancher/v2.5/en/cluster-provisioning/hosted-kubernetes-clusters/aks/
```

Step by Step Rancher install
```
https://phoenixnap.com/kb/install-rancher-on-ubuntu
```

## Install Rancher as a container
Ubuntu is installed on Azure and accessible via ssh on its public IP.
Install Docker
```
sudo apt install docker.io
```
Enable it
```
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
```

We will proceed with the default installation with self-signed certificate.
Install Rancher
```
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  --privileged \
  rancher/rancher:latest
```

##Access Rancher Web Page
Use your Azure public IP with port 80.

##Rancher WebUI
First you will be ask to generate your bootstrap password
