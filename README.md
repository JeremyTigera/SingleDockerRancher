# SingleDockerRancher
How to install and configure a single Rancher server node with Docker in Ubuntu

## Requirements
- Linux Server (in this scenario I use Ubuntu in Azure)
- Docker application installed via Aptitude
- Rancher Server container
- A cloud or vSphere environment for K8s deployement and management (I use Azure)

## Resources
Install Ubuntu server on Azure<br/>
https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal

Install Docker on Ubuntu<br/>
https://docs.docker.com/engine/install/ubuntu/

Deploy Rancher as a container<br/>
https://rancher.com/docs/rancher/v2.5/en/installation/other-installation-methods/single-node-docker/

Create an AKS cluster via Rancher<br/>
https://rancher.com/docs/rancher/v2.5/en/cluster-provisioning/hosted-kubernetes-clusters/aks/

Step by Step Rancher install<br/>
https://phoenixnap.com/kb/install-rancher-on-ubuntu

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
First you will be ask to generate your bootstrap password

![image](https://user-images.githubusercontent.com/101111449/161516582-7747bcc4-e8f3-43d3-8285-0f499b96fa77.png)

## Integrate AKS in Rancher

### Create the Service Principal in AKS
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

Now we need to add a role for this service account.
- Go in **Subscription**
- Click Access Control (**IAM**).
- In the Add role assignment section, click **Add**.
- In the Role field, select a role that will have access to AKS. For example, you can use the **Contributor** role, which has permission to manage everything except for giving access to other users.
- In the Assign access to field, select **service principal**.
- In the Select field, select the name of your **service principal** and click Save.

### Add the Service Principal in Rancher

- Select **Cluster Management** > **Cloud Credentials** > **Create** > **Azure**
![image](https://user-images.githubusercontent.com/101111449/161957163-0d1d9fc5-d084-4775-bf1f-02977738da69.png)

Use your subscription ID, tenant ID, client ID, and client secret to give your cluster access to AKS. If you don’t have all of that information, you can retrieve it using these instructions:

- **Tenant ID**: To get the Tenant ID, you can go to the Azure Portal, then click Azure Active Directory, then click Properties and find the Tenant ID field.
- **Client ID**: To get the Client ID, you can go to the Azure Portal, then click Azure Active Directory, then click Enterprise applications. Click All applications. Select your application, click Properties, and copy the application ID.
- **Client secret**: If you didn’t copy the client secret when creating the service principal, you can get a new one if you go to the app registration detail page, then click Certificates & secrets, then click New client secret.
- **Subscription ID**: You can get the subscription ID is available in the portal from All services > Subscriptions.
