# k8s-build-guide

Building Ansible Playbook to install k8s in my home lab


* Ubuntu OS 

* Longhorn is a distributed block storage system for Kubernetes. Longhorn is cloud-native storage built using Kubernetes and container primitives. Longhorn is lightweight, reliable, and powerful. You can install Longhorn on an existing Kubernetes cluster with one kubectl apply command or by using Helm charts.

* MetalLB is a load-balancer implementation for bare metal Kubernetes clusters, using standard routing protocols. MetalLB is a young project. You should treat it as a beta system.


## Deploy Ubuntu VM's

## Setup SSH 
Open Terminal on your Mac by pressing Command + Space to open Spotlight Search, then type "Terminal" and press Enter.
Generate a new SSH key pair by typing the following command and pressing Enter:

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Replace "your_email@example.com (mailto:your_email@example.com)" with your email address. You can leave the passphrase empty or enter a passphrase for added security.

Copy the public key to your Ubuntu servers using the ssh-copy-id command. Replace "user" with your Ubuntu server username and "your_server_ip" with the IP address of your Ubuntu server:


```
ssh-copy-id -i ~/.ssh/id_ed25519.pub smaniak@master.maniak.lab
ssh-copy-id -i ~/.ssh/id_ed25519.pub smaniak@node1.maniak.lab
ssh-copy-id -i ~/.ssh/id_ed25519.pub smaniak@node2.maniak.lab

```

## Setup SSH Connection

Test the SSH connection by typing the following command and pressing Enter:

```
ssh user@your_server_ip
```

* Edit sudo vi  ~/.ssh/config

```
Host master.maniak.lab
    HostName master.maniak.lab
    User smaniak

Host node1.maniak.lab
    HostName node1.maniak.lab
    User smaniak

Host node2.maniak.lab
    HostName node2.maniak.lab
    User smaniak
```

You should now be able to connect to your Ubuntu servers via SSH from your Mac without a password.

* SSH into each server and make your user the username

```
sudo usermod -aG sudo smaniak
echo "smaniak ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/smaniak
```

## Create your inventory file 

Create a file called hosts.ini, here is an example of mine

```
[master] 
master.maniak.lab

[workers]
node1.maniak.lab
node2.maniak.lab
```
