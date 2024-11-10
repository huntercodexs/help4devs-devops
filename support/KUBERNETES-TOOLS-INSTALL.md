
# KUBERNETES TOOLS INSTALL

> The current Kubernetes version for this tutorial is 1.30.xx

### Pre Requisites

> Hardware

- 4GB RAM
- 16GB HD
- 2 CPU Processor
- Ubuntu 20.04
- Almalinux 8.10

> Software

- Docker: Check the link https://github.com/huntercodexs/help4devs-devops/blob/commons/support/DOCKER-HELPER.md
- Kubectl, Kubeadm, Kubelet

### Ubuntu 20.04

###### Kubernetes Tools Installing

- System Update

<pre>
sudo apt-get update
</pre>

- Install Kubernetes tools

> Option 1: Install kubectl binary using curl

<pre>
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo chmod +x ./kubectl -R

sudo install -o ubuntu-vbox -g ubuntu-vbox -m 0755 kubectl /usr/local/bin/kubectl
</pre>

> Option 2: Install all kubernetes tools using native package management

<pre>
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list
</pre>

<pre>
sudo apt-get update

sudo apt-get install kubeadm

sudo apt-get install kubelet

sudo apt-get install kubectl
</pre>

- Mark the Kubernetes as hold (optional)

<pre>
sudo apt-mark hold kubeadm kubelet kubectl
</pre>

- Check Kubernetes version

<pre>
kubeadm version

kubelet --version

kubectl version --client
</pre>

- Turn off the Swap for Kubernetes

<pre>
sudo swapoff –a
</pre>

- Set the Unique Server Node

<pre>
sudo hostnamectl set-hostname master-node
</pre>

- Set the Workers machine, for example:

<pre>
sudo hostnamectl set-hostname work-name-1
</pre>

- Start Kubernetes on Server Node

<pre>
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
</pre>

Create the folder and the configuration file for kubernetes

<pre>
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
</pre>

The config file should have the following content

<pre>
apiVersion: v1
kind: Config
preferences: {}
</pre>

- Deploy POD Network to Cluster

<pre>
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
</pre>

- Check if everything is ok

<pre>
kubectl get pods --all-namespaces
</pre>

- Join the workers node to cluster

<pre>
kubeadm join --discovery-token abcdef.1234567890abcdef --discovery token-ca-cert-hash sha256:1234..cdef 1.2.3.4:6443
</pre>

###### Uninstalling

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 8080"
#sudo ufw delete allow 8080/tcp
#sudo ufw reload

sleep 2

echo "Stopping and removing service - kubernetes.service"
#sudo systemctl stop kubernetes.service
#sudo service kubernetes stop
#sudo rm -f /etc/systemd/system/kubernetes.service
#sudo rm -f /etc/systemd/system/multi-user.target.wants/kubernetes.service
#sudo systemctl daemon-reload

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
sudo rm -rf $HOME/.kube
sudo find . | grep -v home | grep kube | xargs -i sudo rm -rf {} >> /dev/null 2>&1

sleep 2

echo "Removing user - kubernetes"
sudo userdel kubernetes
cd /home/ubuntu-vbox

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

###### Installing

- System Update

<pre>
sudo yum update
sudo dnf update
</pre>

- Install Kubernetes tools

> Option 1: Install kubectl binary with curl on Linux

<pre>
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo yum add
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo chmod +x ./kubectl -R
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
</pre>

> Option 2: Install using native package management

<pre>
sudo vi /etc/yum.repos.d/kubernetes.repo
</pre>

<pre>
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.30/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.30/rpm/repodata/repomd.xml.key
</pre>

<pre>
sudo dnf install kubectl
sudo dnf install kubeadm kubelet kubectl --disableexcludes=kubernetes
</pre>

- Create the configuration file

<pre>
sudo vi /var/lib/kubelet/config.yaml
</pre>

<pre>
apiVersion: kubelet.config.k8s.io/v1beta1
#authentication:
#  anonymous:
#    enabled: false
#  webhook:
#    cacheTTL: 0s
#    enabled: true
#  x509:
#    clientCAFile: /etc/kubernetes/pki/ca.crt
#authorization:
#  mode: Webhook
#  webhook:
#    cacheAuthorizedTTL: 0s
#    cacheUnauthorizedTTL: 0s
#cgroupDriver: systemd
#clusterDNS:
#- 10.0.0.0
#clusterDomain: cluster.local
#cpuManagerReconcilePeriod: 0s
#evictionPressureTransitionPeriod: 0s
#fileCheckFrequency: 0s
#healthzBindAddress: 127.0.0.1
#healthzPort: 10248
#httpCheckFrequency: 0s
#imageMinimumGCAge: 0s
#kind: KubeletConfiguration
#logging: {}
#nodeStatusReportFrequency: 0s
#nodeStatusUpdateFrequency: 0s
#rotateCertificates: true
#runtimeRequestTimeout: 0s
#shutdownGracePeriod: 0s
#shutdownGracePeriodCriticalPods: 0s
#staticPodPath: /etc/kubernetes/manifests
#streamingConnectionIdleTimeout: 0s
#syncFrequency: 0s
#volumeStatsAggPeriod: 0s
</pre>

<pre>
sudo vi /etc/kubernetes/bootstrap-kubelet.conf
</pre>

<pre>
....
</pre>

- Enable Kubernetes services

<pre>
sudo systemctl daemon-reload
sudo systemctl enable kubelet
sudo systemctl stop kubelet
sudo systemctl start kubelet
sudo systemctl status kubelet
</pre>

- Install KIND

<pre>
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
sudo chmod +x ./kind -R
sudo mv ./kind /usr/local/bin/kind
</pre>

###### Uninstalling

> TIP: You can create a script to automate this process, for example destroy.sh

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Locking firewall - port 8080"
#sudo ufw delete allow 8080/tcp
#sudo ufw reload

sleep 2

echo "Stopping and removing service - kubernetes.service"
#sudo systemctl stop kubernetes.service
#sudo service kubernetes stop
#sudo rm -f /etc/systemd/system/kubernetes.service
#sudo rm -f /etc/systemd/system/multi-user.target.wants/kubernetes.service
#sudo systemctl daemon-reload

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
sudo rm -r $HOME/.kube/
sudo find . | grep -v home | grep kube | xargs -i sudo rm -rf {} >> /dev/null 2>&1

sleep 2

echo "Removing user - kubernetes"
sudo userdel kubernetes
cd /home/ubuntu-vbox

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
