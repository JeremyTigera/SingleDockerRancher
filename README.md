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

## Access Rancher Web Page
Use your Azure public IP with port 80.

## Rancher WebUI
First you will be ask to generate your bootstrap password

![image](https://user-images.githubusercontent.com/101111449/161516582-7747bcc4-e8f3-43d3-8285-0f499b96fa77.png)

## Integrate AKS in Rancher
In order to deploy and manage an AKS Cluster within Rancher, we need to create the credentials in Azure and configure them in Rancher.

- Go to the Microsoft **Azure Portal home** page.
- Click **Azure Active Directory**.
- Click **App registrations**.
- Click **New registration**.
- **Enter a name**. This will be the name of your service principal.
- Click **Register**.

You should now see the name of your service principal under Azure Active Directory > App registrations.

- Click the name of your service principal. Take note of the **tenant ID** and **application ID** (also called **app ID** or **client ID**) so that you can use it when provisioning your AKS cluster.
- Then click C**ertificates & secrets**.
- Click **New client secret**. Enter a short description, pick an expiration time, and click **Add**. Take note of the **client secret** so that you can use it when provisioning the AKS cluster.
