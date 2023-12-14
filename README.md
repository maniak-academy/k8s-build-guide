# k8s-build-guide

Building Ansible Playbook to install k8s in my home lab

## Setup SSH 
Open Terminal on your Mac by pressing Command + Space to open Spotlight Search, then type "Terminal" and press Enter.
Generate a new SSH key pair by typing the following command and pressing Enter:

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Replace "your_email@example.com (mailto:your_email@example.com)" with your email address. You can leave the passphrase empty or enter a passphrase for added security.

Copy the public key to your Ubuntu servers using the ssh-copy-id command. Replace "user" with your Ubuntu server username and "your_server_ip" with the IP address of your Ubuntu server:

```
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@your_server_ip
```

Test the SSH connection by typing the following command and pressing Enter:

```
ssh user@your_server_ip
```

You should now be able to connect to your Ubuntu servers via SSH from your Mac without a password.


echo "smaniak ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/smaniak




Seb's guide for ubuntu

Prerequisites: Ensure that you have a clean Ubuntu 22.04 installation on all three machines. The machines should have at least 2GB of RAM and 2 CPUs each. Also, make sure you have a user with sudo privileges on all machines.
Update and Upgrade: On all machines, run the following commands to update and upgrade the system packages:


Disable Swap Permanently

To ensure swap remains disabled after reboots, you'll need to edit the /etc/fstab file to comment out the swap line:
Open the file with a text editor like nano:
bash

sudo nano /etc/fstab

Find the line that looks something like this (it may vary):


/swapfile swap swap defaults 0 0

Comment it out by adding a # at the beginning of the line:


#/swapfile swap swap defaults 0 0

Save and close the file (in nano, it's Ctrl+O, Enter, Ctrl+X).
Reboot the System

After making this change, reboot your system to ensure all changes are applied and swap is completely disabled:

sudo reboot


install Docker

```
sudo apt update && sudo apt upgrade -y
```

```
sudo apt install docker.io -y
```

sudo systemctl enable docker
sudo systemctl start docker



Install Kubernetes Components: Install the necessary Kubernetes components on all three machines:
bash
sudo apt install curl apt-transport-https -y

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt-get update
sudo apt update

sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

initate on master node

```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```


#run on nodes 

```
kubeadm join 172.16.10.200:6443 --token xi97p6.0cqxvtb1gq2d1vrx --discovery-token-ca-cert-hash sha256:00ee806e931229fc5c07057aa2399756d3ad7409826825a87d5eb87d0190fd45 
```

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml


Set up local kubeconfig:

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```



sudo -i
swapoff -a
exit
strace -eopenat kubectl version