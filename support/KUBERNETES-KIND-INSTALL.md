
# KUBERNETES KIND INSTALL

> The current Kubernetes version for this tutorial is 1.30.xx

> KIND means "Kubernetes In Docker"

> This tutorial consider using the KIND with local installation
> and consider a local environment do simulate a production set
> <br />
> Source: https://kind.sigs.k8s.io/

### Pre Requisites

> Hardware

- 4GB RAM
- 16GB HD
- 2 CPU Processor
- Ubuntu 20.04
- Almalinux 8.10

> Software

- Docker: Check the link https://github.com/huntercodexs/help4devs/blob/commons/support/DOCKER-HELPER.md
- Kubectl, Kubeadm, Kubelet
- KIND

### Ubuntu 20.04

###### KIND Installing

<pre>
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
sudo chmod +x ./kind -R
sudo cp ./kind /usr/local/bin/kind
</pre>

###### Kubectl Installing

- System Update

<pre>
sudo apt-get update
</pre>

<pre>
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo chmod +x ./kubectl -R

sudo install -o ubuntu-vbox -g ubuntu-vbox -m 0755 kubectl /usr/local/bin/kubectl
</pre>

- Check Kubectl version

<pre>
kubectl version --client
</pre>

###### Uninstalling

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

sleep 2

echo "Removing kubernetes, kind, folders, and links"
cd /
sudo rm -rf /usr/local/bin/kind
sudo rm -rf /usr/local/bin/kubectl

sleep 2

echo ""
echo -n "Removing Docker Containers ? Continue[Y], Skip[N]: "
read RMDOCKER

if [[ "${RMDOCKER}" == "Y" ]]; 
then 
    docker system prune -a
else
    echo "OK - Skipping"
fi
echo ""

echo "DONE"
echo "Maybe it will be necessary to restart the machine."
exit
</pre>

### Almalinux 8.10

###### KIND Installing

<pre>
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
sudo chmod +x ./kind -R
sudo cp ./kind /usr/local/bin/kind
</pre>

###### Kubectl Installing

- System Update

<pre>
sudo apt-get update
</pre>

<pre>
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo chmod +x ./kubectl -R

sudo install -o ubuntu-vbox -g ubuntu-vbox -m 0755 kubectl /usr/local/bin/kubectl
</pre>

- Check Kubectl version

<pre>
kubectl version --client
</pre>

###### Uninstalling

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

sleep 2

echo "Removing kubernetes, kind, folders, and links"
cd /
sudo rm -rf /usr/local/bin/kind
sudo rm -rf /usr/local/bin/kubectl

sleep 2

echo ""
echo -n "Removing Docker Containers ? Continue[Y], Skip[N]: "
read RMDOCKER

if [[ "${RMDOCKER}" == "Y" ]]; 
then 
    docker system prune -a
else
    echo "OK - Skipping"
fi
echo ""

echo "DONE"
echo "Maybe it will be necessary to restart the machine."
exit
</pre>