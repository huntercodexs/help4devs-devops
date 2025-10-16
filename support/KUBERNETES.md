# KUBERNETES TOOLS INSTALL

> NOTE
> The current Kubernetes version for this tutorial is v1.31.2
> Kind releases compatibles https://github.com/kubernetes-sigs/kind/releases
> Minikube https://storage.googleapis.com/minikube/releases/v1.31.2/minikube-linux-amd64
> Check the https://kubernetes.io/releases/download/

## Pre Requisites

> Hardware

- 4GB RAM
- 32GB HD
- 2 CPU Processor

> Software

- Ubuntu 20.04
- Docker (see https://github.com/huntercodexs/help4devs-devops/blob/commons/support/DOCKER-HELPER.md)
- Docker Compose
- Kubernetes
- Kubectl
- Kubelet
- Kubeadm
- Kind
- Minikube
- Jenkins

## Installing

### System Update

<pre>
sudo apt-get update
</pre>

### Kubernetes Tools

- Kubectl

> Option 1: Kubectl (alone)

Install only kubectl binary using curl

<pre>
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo chmod +x ./kubectl -R
sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 kubectl /usr/local/bin/kubectl
</pre>

> Option 2: Kubectl + Kubelet + Kubeadm

Set the environment for all kubernetes tools using native package management (also applied for kubelet and kubeadm)

<pre>
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings/
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31.2/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31.2/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
</pre>

***Kubectl***

<pre>
sudo apt-get install kubectl
kubectl version --client
</pre>

***Kubelet***

<pre>
sudo apt-get install kubelet
kubelet --version
</pre>

***Kubeadm***

<pre>
sudo apt-get install kubeadm
kubeadm version
</pre>

> Option 3: All Kubernetes tools

Install all kubernetes tools using the direct link from kubernetes.io

<pre>
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

wget https://dl.k8s.io/v1.31.2/bin/linux/amd64/kubectl
wget https://dl.k8s.io/v1.31.2/bin/linux/amd64/kubelet
wget https://dl.k8s.io/v1.31.2/bin/linux/amd64/kubeadm
wget https://dl.k8s.io/v1.31.2/bin/linux/amd64/kube-proxy
wget https://dl.k8s.io/v1.31.2/bin/linux/amd64/kubectl-convert
wget https://dl.k8s.io/v1.31.2/bin/linux/amd64/kube-apiserver
wget https://dl.k8s.io/v1.31.2/bin/linux/amd64/mounter
wget https://dl.k8s.io/v1.31.2/bin/linux/amd64/kube-controller-manager
wget https://dl.k8s.io/v1.31.2/bin/linux/amd64/kube-aggregator
wget https://dl.k8s.io/v1.31.2/bin/linux/amd64/apiextensions-apiserver

sudo chmod +x ./kubectl -R
sudo chmod +x ./kubelet -R
sudo chmod +x ./kubeadm -R
sudo chmod +x ./kube-proxy -R
sudo chmod +x ./kubectl-convert -R
sudo chmod +x ./kube-apiserver -R
sudo chmod +x ./mounter -R
sudo chmod +x ./kube-controller-manager -R
sudo chmod +x ./kube-aggregator -R
sudo chmod +x ./apiextensions-apiserver -R

sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 kubectl /usr/local/bin/kubectl
sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 kubelet /usr/local/bin/kubelet
sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 kubeadm /usr/local/bin/kubeadm
sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 kube-proxy /usr/local/bin/kube-proxy
sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 kubectl-convert /usr/local/bin/kubectl-convert
sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 kube-apiserver /usr/local/bin/kube-apiserver
sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 mounter /usr/local/bin/kube-mounter
sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 kube-controller-manager /usr/local/bin/kube-controller-manager
sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 kube-aggregator /usr/local/bin/kube-aggregator
sudo install -o ubuntu-kube-vbox -g ubuntu-kube-vbox -m 0755 apiextensions-apiserver /usr/local/bin/kube-apiextensions-apiserver

kubectl version --client
kubelet --version
kubeadm version
kube-proxy --version
kube-apiserver --version
kube-controller-manager --version
</pre>

- Kind

<pre>
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.25.0/kind-linux-amd64
sudo chmod +x ./kind -R
sudo cp ./kind /usr/local/bin/kind
kind --version
</pre>

- Minikube

<pre>
curl -LO https://storage.googleapis.com/minikube/releases/v1.31.2/minikube-linux-amd64
mv minikube-linux-amd64 minikube
chmod +x ./minikube
sudo install minikube /usr/local/bin/minikube
minikube version
</pre>

- Mark the Kubernetes as hold (optional)

<pre>
sudo apt-mark hold kubeadm kubelet kubectl
</pre>

- Turn off the Swap for Kubernetes

<pre>
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
</pre>

- Set the Unique Server Node (master node)

<pre>
sudo hostnamectl set-hostname ubuntu-kube-vbox
</pre>

<pre>
ubuntu-kube-vbox@ubuntu-kube-vbox:~/Kubernetes$ hostnamectl 
   Static hostname: ubuntu-kube-vbox
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 4bfde4c352904115b966f18717d167cf
           Boot ID: 20802a9552ee4c53b2af67294fb685bc
    Virtualization: oracle
  Operating System: Ubuntu 20.04.6 LTS
            Kernel: Linux 5.15.0-125-generic
      Architecture: x86-64
</pre>

## Managing

## Docker Build

- Docker User Group

> NOTE: Maybe it will be necessary to restart the machine or VM

<pre>
sudo usermod -aG docker ${USER}
su - ${USER}
</pre>

- Docker image build and publish

<pre>
docker login -u huntercodexs
docker build --no-cache -t spring-boot-demo:latest .
docker run -d -p 38087:38087 -it spring-boot-demo:latest
docker tag spring-boot-demo:latest huntercodexs/spring-boot-demo:latest
docker push huntercodexs/spring-boot-demo:latest 
</pre>

### Minikube

- Start the minikube service

<pre>
minikube start
</pre>

***Result***

<pre>
😄  minikube v1.31.2 on Ubuntu 20.04 (vbox/amd64)
🎉  minikube 1.34.0 is available! Download it: https://github.com/kubernetes/minikube/releases/tag/v1.34.0
💡  To disable this notice, run: 'minikube config set WantUpdateNotification false'

✨  Automatically selected the docker driver
📌  Using Docker driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.27.4 preload ...
    > preloaded-images-k8s-v18-v1...:  393.21 MiB / 393.21 MiB  100.00% 27.57 M
    > gcr.io/k8s-minikube/kicbase...:  447.62 MiB / 447.62 MiB  100.00% 11.03 M
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.27.4 on Docker 24.0.4 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass

❗  /usr/local/bin/kubectl is version 1.31.2, which may have incompatibilities with Kubernetes 1.27.4.
    ▪ Want kubectl v1.27.4? Try 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
</pre>

### Kubectl

- Create a service account in Kubernetes

<pre>
kubectl create serviceaccount jenkins
</pre>

- Generating Token for the service account

<pre>
openssl rand -base64 32
example output: ei6rSR1xW9ctVOLNrYykXMWQF2dgurlJ6YhJZjmWZ7o=
</pre>

<pre>
kubectl create secret generic jenkins-token --from-literal=token=ei6rSR1xW9ctVOLNrYykXMWQF2dgurlJ6YhJZjmWZ7o=
</pre>

- Associate the token with the kubernetes service account

<pre>
kubectl patch serviceaccount jenkins -p '{"imagePullSecrets": [{"name": "jenkins-token"}]}'
</pre>

- Finding the Minikube URL

> IMPORTANT: This url and port need to be used in the pipeline file
> DON'T STOP THIS SERVICE UNTIL YOU DEPLOY THE APPLICATION

<pre>
ubuntu-kube-vbox@ubuntu-kube-vbox:~/Transfer/spring-boot-demo$ minikube dashboard --url
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
http://127.0.0.1:45377/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
</pre>

***Result***

<pre>
🔌  Enabling dashboard ...
    ▪ Using image docker.io/kubernetesui/dashboard:v2.7.0
    ▪ Using image docker.io/kubernetesui/metrics-scraper:v1.0.8
💡  Some dashboard features require the metrics-server addon. To enable all features please run:

	minikube addons enable metrics-server	

🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
http://127.0.0.1:36751/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
</pre>

- Check the Minikube status

<pre>
ubuntu-kube-vbox@ubuntu-kube-vbox:~$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
</pre>

- Jenkins Integration

<pre>
Install Plugins: git, docker, kubernetes
Create Credentials: Secret text
Create new Item: Pipeline
</pre>

- Create namespace and deploy

<pre>
kubectl create namespace spring-boot-demo--namespace
kubectl apply -f deployment.yaml -n spring-boot-demo--namespace
kubectl apply -f service.yaml -n spring-boot-demo--namespace
kubectl apply -f role.yaml -n spring-boot-demo--namespace
</pre>

- Get pods

<pre>
eval $(minikube docker-env)
kubectl get pods -n spring-boot-demo--namespace
</pre>

- Get node access when using minikube

<pre>
minikube tunnel
kubectl get services -n spring-boot-demo--namespace
</pre>

Use the IP and port to access the application running inside the pod, for example:
http://10.101.15.145:8087/hello

## Uninstalling

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

CLUSTER=$1

if [[ !"$CLUSTER" || "$CLUSTER" == "" ]];
then
    echo ""
    echo "WARNING: There is not clusters in the parameters !"
    echo ""

    CLUSTER=""
fi

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Stopping minikube, kind, kubectl and services"
minikube stop
minikube remove
minikube delete
kind delete cluster
kind delete cluster $CLUSTER
kubectl delete cluster
kubectl delete cluster $CLUSTER

sleep 2

echo "Locking firewall - port 8080"
sudo ufw delete allow 80/tcp
sudo ufw delete allow 8080/tcp
sudo ufw delete allow 30000/tcp
sudo ufw delete allow 38080/tcp
sudo ufw delete allow 38087/tcp
sudo ufw reload

sleep 2

echo "Stopping and removing service - kubernetes.service"
sudo systemctl stop kubelet
sudo systemctl stop kubernetes
sudo rm -f /etc/systemd/system/kubernetes.service
sudo rm -f /etc/systemd/system/multi-user.target.wants/kubernetes.service
sudo systemctl daemon-reload

sleep 2

echo "Removing kubernetes installations"
sudo apt remove kubernetes* kubectl kubeadm kubelet -y
sudo apt-get remove kubernetes -y
sudo apt-get remove kubectl -y
sudo apt-get remove kubeadm -y
sudo apt-get remove kubelet -y
sudo apt-get remove --auto-remove -y
sudo dpkg --purge kubernetes

sleep 2

echo "Removing kubernetes, kind, folders, and links"
cd /
sudo rm -rf /etc/apt/sources.list.d/kubernetes.list
sudo rm -rf /etc/kubernetes/

sudo rm -rf /usr/local/bin/kind
sudo rm -rf /usr/local/bin/minikube
sudo rm -rf /usr/local/bin/kubectl
sudo rm -rf /usr/local/bin/kubelet
sudo rm -rf /usr/local/bin/kubeadm
sudo rm -rf /usr/local/bin/kube-proxy
sudo rm -rf /usr/local/bin/kubectl-convert
sudo rm -rf /usr/local/bin/kube-apiserver
sudo rm -rf /usr/local/bin/kube-mounter
sudo rm -rf /usr/local/bin/kube-controller-manager
sudo rm -rf /usr/local/bin/kube-aggregator
sudo rm -rf /usr/local/bin/kube-apiextensions-apiserver

sudo rm -rf /home/$USER/.kube
sudo rm -rf /home/$USER/.kind
sudo rm -rf /home/$USER/.minikube
sudo rm -rf /home/$USER/.kubectl
sudo rm -rf /home/$USER/.kubelet
sudo rm -rf /home/$USER/.kubeadm
sudo rm -rf /home/$USER/.kube-proxy
sudo rm -rf /home/$USER/.kubectl-convert
sudo rm -rf /home/$USER/.kube-apiserver
sudo rm -rf /home/$USER/.kube-mounter
sudo rm -rf /home/$USER/.kube-controller-manager
sudo rm -rf /home/$USER/.kube-aggregator
sudo rm -rf /home/$USER/.kube-apiextensions-apiserver

sudo find . | grep -v home | grep kube | xargs -i sudo rm -rf {} >> /dev/null 2>&1

sleep 2

echo "Removing user - kubernetes"
sudo userdel kubernetes
cd /home/ubuntu-kube-vbox
sudo swapon -a

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
