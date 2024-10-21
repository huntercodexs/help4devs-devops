
# KUBERNETES KIND HELPER

> NOTE: These tutorial were created and tested using Virtual Machines via Virtual Box 

### KIND + KUBECTL

###### KIND: Create Cluster

<pre>
kind create cluster
</pre>

Also, you can specify a cluster name

<pre>
kind create cluster --name ${CLUSTER-NAME}
</pre>

> NOTE: This process create a .kube folder in the home directory for the current user

###### KIND: Check Cluster

<pre>
kind get clusters
</pre>

<pre>
kind
my-first-cluster
</pre>

###### KIND: Delete Cluster

<pre>
kind delete cluster
</pre>

<pre>
Deleting cluster "kind" ...
Deleted nodes: ["kind-control-plane"]
</pre>

###### KUBECTL: Set Cluster

When we have more than one cluster, we can change the current cluster as default

<pre>
kubectl config set-cluster ${CLUSTER-NAME}
</pre>

###### KUBECTL: Check nodes

<pre>
kubectl get nodes
</pre>

<pre>
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   15m   v1.30.0
</pre>

<pre>
# Get all information about the clusters
kubectl get all -A
</pre>

###### DOCKER: Check Container

<pre>
docker container ls
</pre>

<pre>
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                       NAMES
8e5ca5d25b15   kindest/node:v1.30.0   "/usr/local/bin/entr…"   9 minutes ago   Up 9 minutes   127.0.0.1:33069->6443/tcp   kind-control-plane
</pre>

### KIND: Reset Clusters

> WARNING: This step will erase everything in the current kube configuration

You can use the code block below to create a script and automate the process to destroy the 
kind and kubectl installation and configurations

<pre>
#!/bin/bash

echo "This process can't to be undo, Continue ?"
echo "Press [Enter] to continue, Press [Ctrl+C] to Abort "
read OP

echo "Removing clusters"
kind delete cluster

echo ""
echo "Removing specific cluster"
kind get clusters

echo ""
echo "Inform the cluster name, Press [Enter] to ignore: "
read CLUSTER_NAME

if [[ "${CLUSTER_NAME}" != "" ]];
then
    kind delete cluster --name ${CLUSTER_NAME}
    kind get clusters
fi

echo ""
echo "Removing folder and paths"
echo '' > ~/.kube/config
rm -rf ~/.kube/cache

sleep 2

echo ""
echo "Locking firewall"
sudo ufw status
sudo ufw delete allow 80/tcp
sudo ufw delete allow 8080/tcp
sudo ufw delete allow 30000/tcp
sudo ufw reload
sudo ufw status

sleep 2

echo ""
echo "DONE"
exit
</pre>

### Samples

###### KUBECTL: DEPLOYMENT (SAMPLE 1)

- Create the YAML file configuration, like this:

<pre>
vi ~/Kubernetes/deployment-nginx.yaml
</pre>

<pre>
apiVersion: v1
kind: Deployment
metadata:
  name: nginx
spec:
  selector:
    matchLabels:
      app: nginx
    template:
      metadata:
        labels:
          app: nginx
      spec:
        containers:
        - name: nginx
          image: nginx
          ports:
          - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
</pre>

- Allow ports in the firewall

<pre>
sudo ufw allow 8080/tcp
sudo ufw allow 80/tcp
sudo ufw allow 'Nginx Full'
sudo ufw reload
</pre>

- Make deployment

<pre>
kubectl apply -f ${CONFIGURATION-FILE-YAML}

#Example
kubectl apply -f /home/${USER}/Kubernetes/deployment-nginx.yaml
</pre>

- Get Pods

<pre>
kubectl get pod
</pre>

[Example]

<pre>
kubectl get pod
NAME                     READY   STATUS    RESTARTS   AGE
nginx-6cfb64b7c5-xtwdw   1/1     Running   0          40s
</pre>

- Get Service

<pre>
kubectl get svc
</pre>

[Example]

<code>

    kubectl get svc
    NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
    kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP   90m
    nginx        ClusterIP   10.96.98.116   <none>        80/TCP    6m39s

</code>

- Redirect request to service port - Forward

<pre>
kubectl port-forward svc/nginx 8080:80
</pre>

- Test the application running in the kubernetes

<pre>
curl http://localhost:8080
</pre>

###### KUBECTL: DEPLOYMENT (SAMPLE 2 - Multi-Node)

> NOTE: Reset the Kubernetes and KIND state
> <br />
> Reference: https://kind.sigs.k8s.io/docs/user/quick-start/#configuring-your-kind-cluster

- Get the configuration content and put in the yaml configuration file

[Example]

<pre>
apiVersion: kind.x-k8s.io/v1alpha4
kind: Cluster
nodes:
  - role: control-plane
  - role: worker
  - role: worker
</pre>

In the above case we can see a complete cluster configuration using multi-node as format, where it's 
possible to realize that there are another configurations that means the control-plane and workers.

- Create the cluster

<pre>
kind create cluster --name my-multinode-cluster --config /home/${USER}/Kubernetes/multi-node-kind-example-config.yaml
</pre>

[Example]

<pre>
kind create cluster --name my-multinode-cluster --config /home/${USER}/Kubernetes/multi-node-kind-example-config.yaml
Creating cluster "my-multinode-cluster" ...
 ✓ Ensuring node image (kindest/node:v1.30.0) 🖼
 ✓ Preparing nodes 📦 📦 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
 ✓ Joining worker nodes 🚜 
Set kubectl context to "kind-my-multinode-cluster"
You can now use your cluster with:

kubectl cluster-info --context kind-my-multinode-cluster

Thanks for using kind! 😊
</pre>

- Check nodes created

<pre>
kubectl get nodes
</pre>

[Example]

<code>

    kubectl get nodes
    NAME                                 STATUS   ROLES           AGE   VERSION
    my-multinode-cluster-control-plane   Ready    control-plane   74m   v1.30.0
    my-multinode-cluster-worker          Ready    <none>          74m   v1.30.0
    my-multinode-cluster-worker2         Ready    <none>          74m   v1.30.0

</code>

- Deploy the application

<pre>
kubectl apply -f /home/ubuntu-vbox/Kubernetes/deployment-nginx.yaml
</pre>

- Check deployment status

<pre>
kubectl get pod -o wide
</pre>

[Example]

<code>

    kubectl get pod -o wide
    NAME                     READY   STATUS    RESTARTS   AGE   IP           NODE                           NOMINATED NODE   READINESS GATES
    nginx-6cfb64b7c5-zt7qn   1/1     Running   0          55s   10.244.1.2   my-multinode-cluster-worker2   <none>           <none>

</code>

- Create replicas

Change the deployment-nginx.yaml configuration file with something like that

<pre>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
</pre>

And apply the change in the current cluster

<pre>
kubectl apply -f /home/ubuntu-vbox/Kubernetes/deployment-nginx.yaml
</pre>

Now we can see 4 replicas from the nginx container

<pre>
kubectl get po
NAME                     READY   STATUS    RESTARTS   AGE
nginx-6cfb64b7c5-l4rf2   1/1     Running   0          11s
nginx-6cfb64b7c5-zcz8n   1/1     Running   0          11s
nginx-6cfb64b7c5-zpmqc   1/1     Running   0          11s
nginx-6cfb64b7c5-zt7qn   1/1     Running   0          9m38s
</pre>

- Make forward manually to allow access to the pod service

<pre>
kubectl port-forward svc/nginx 8080:80
</pre>

###### KUBECTL: DEPLOYMENT (SAMPLE 3 - High Availability)

> NOTE: Reset the Kubernetes and KIND state
> <br />
> Reference: https://kind.sigs.k8s.io/docs/user/quick-start/#configuring-your-kind-cluster

- Get the configuration content and put in the yaml configuration file

[Example]

<pre>
apiVersion: kind.x-k8s.io/v1alpha4
kind: Cluster
nodes:
  - role: control-plane
  - role: control-plane
  - role: control-plane
  - role: worker
  - role: worker
  - role: worker
  - role: worker
  - role: worker
</pre>

In the above case we can see a complete cluster configuration using Control-plane HA format, where it's
possible to realize that there are another configurations that means the control-plane and workers, but 
now we have three control-planes.

- Create the cluster

<pre>
kind create cluster --name my-control-plane-ha-cluster --config /home/${USER}/Kubernetes/control-plane-ha-kind-example-config.yaml
</pre>

[Example]

<pre>
kind create cluster --name my-control-plane-ha-cluster --config /home/${USER}/Kubernetes/control-plane-ha-kind-example-config.yaml
Creating cluster "my-control-plane-ha-cluster" ...
 ✓ Ensuring node image (kindest/node:v1.30.0) 🖼
 ✓ Preparing nodes 📦 📦 📦 📦 📦 📦 📦 📦  
 ✓ Configuring the external load balancer ⚖️ 
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
 ✓ Joining more control-plane nodes 🎮 
 ✓ Joining worker nodes 🚜 
Set kubectl context to "kind-my-control-plane-ha-cluster"
You can now use your cluster with:

kubectl cluster-info --context kind-my-control-plane-ha-cluster

Not sure what to do next? 😅  Check out https://kind.sigs.k8s.io/docs/user/quick-start/
</pre>

- Check nodes created

<pre>
kubectl get nodes
</pre>

[Example]

Below we have a high availability environment to deploy our applications

<code>

    NAME                                         STATUS   ROLES           AGE   VERSION
    my-control-plane-ha-cluster-control-plane    Ready    control-plane   39m   v1.30.0
    my-control-plane-ha-cluster-control-plane2   Ready    control-plane   39m   v1.30.0
    my-control-plane-ha-cluster-control-plane3   Ready    control-plane   38m   v1.30.0
    my-control-plane-ha-cluster-worker           Ready    <none>          38m   v1.30.0
    my-control-plane-ha-cluster-worker2          Ready    <none>          38m   v1.30.0
    my-control-plane-ha-cluster-worker3          Ready    <none>          38m   v1.30.0
    my-control-plane-ha-cluster-worker4          Ready    <none>          38m   v1.30.0
    my-control-plane-ha-cluster-worker5          Ready    <none>          38m   v1.30.0

</code>

Also, it's possible to see the created containers and mainly the HA-Proxy container that makes the routes 
from any origins into internal cluster destiny.

<pre>
docker container ls
</pre>

[Example]

<pre>
docker container ls
CONTAINER ID   IMAGE                                COMMAND                  CREATED          STATUS          PORTS                       NAMES
6dbeede33257   kindest/haproxy:v20230606-42a2262b   "haproxy -W -db -f /…"   41 minutes ago   Up 41 minutes   127.0.0.1:41091->6443/tcp   my-control-plane-ha-cluster-external-load-balancer
f117fbba9f1e   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   41 minutes ago   Up 41 minutes                               my-control-plane-ha-cluster-worker2
bf479d91db8c   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   41 minutes ago   Up 41 minutes   127.0.0.1:36097->6443/tcp   my-control-plane-ha-cluster-control-plane2
43dc8545d3d7   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   41 minutes ago   Up 41 minutes                               my-control-plane-ha-cluster-worker3
257e16789d0f   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   41 minutes ago   Up 41 minutes                               my-control-plane-ha-cluster-worker4
cfab91ea89c5   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   41 minutes ago   Up 41 minutes   127.0.0.1:32789->6443/tcp   my-control-plane-ha-cluster-control-plane
ad45e86eed06   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   41 minutes ago   Up 41 minutes                               my-control-plane-ha-cluster-worker
8a7715d4700b   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   41 minutes ago   Up 41 minutes                               my-control-plane-ha-cluster-worker5
ed4afd77575f   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   41 minutes ago   Up 41 minutes   127.0.0.1:44385->6443/tcp   my-control-plane-ha-cluster-control-plane3
</pre>

- Deploy the application

<pre>
kubectl apply -f /home/ubuntu-vbox/Kubernetes/deployment-nginx.yaml
</pre>

- Check deployment status

<pre>
kubectl get pod -o wide
</pre>

[Example]

> This example consider that you are using replicas=4 in the yaml file configuration to deploy

<code>

    NAME                     READY   STATUS    RESTARTS   AGE   IP           NODE                                  NOMINATED NODE   READINESS GATES
    nginx-6cfb64b7c5-4qd62   1/1     Running   0          29s   10.244.3.2   my-control-plane-ha-cluster-worker5   <none>           <none>
    nginx-6cfb64b7c5-6pwht   1/1     Running   0          29s   10.244.5.2   my-control-plane-ha-cluster-worker3   <none>           <none>
    nginx-6cfb64b7c5-8p2vr   1/1     Running   0          62s   10.244.6.2   my-control-plane-ha-cluster-worker2   <none>           <none>
    nginx-6cfb64b7c5-gxvrf   1/1     Running   0          29s   10.244.7.2   my-control-plane-ha-cluster-worker    <none>           <none>

</code>

- Make forward automate

Open the deployment-nginx.yaml file to add the following information

<pre>
vi /home/${USER}/Kubernetes/deployment-nginx.yaml
</pre>

<pre>
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30000
  type: NodePort
</pre>

Apply the changes 

<pre>
kubectl apply -f /home/${USER}/Kubernetes/deployment-nginx.yaml
</pre>

###### KUBECTL: DEPLOYMENT (SAMPLE 4 - High Availability + Ports Mapping)

> NOTE: Reset the Kubernetes and KIND state
> <br />
> Reference: https://kind.sigs.k8s.io/docs/user/quick-start/#configuring-your-kind-cluster

- Get the configuration content and put in the yaml configuration file as follows

[Example]

Change the yaml file with something like below

<pre>
vi /home/${USER}/Kubernetes/control-plane-ha-kind-example-config.yaml
</pre>

<pre>
apiVersion: kind.x-k8s.io/v1alpha4
kind: Cluster
nodes:
  - role: control-plane
  - role: control-plane
  - role: control-plane
  - role: worker
    extraPortMappings:
      - containerPort: 30000
        hostPort: 8080
        #listenAddress: "0.0.0.0" # Optional, defaults to "0.0.0.0"
        #protocol: tcp # Optional, defaults to tcp
  - role: worker
  - role: worker
</pre>

- Create the clusters

<pre>
kind create cluster --name my-control-plane-ha-port-map --config /home/${USER}/Kubernetes/control-plane-ha-kind-example-config.yaml
</pre>

[Example]

<pre>
kind create cluster --name my-control-plane-ha-port-map --config /home/${USER}/Kubernetes/control-plane-ha-kind-example-config.yaml
Creating cluster "my-control-plane-ha-port-map" ...
 ✓ Ensuring node image (kindest/node:v1.30.0) 🖼
 ✓ Preparing nodes 📦 📦 📦 📦 📦 📦 
 ✓ Configuring the external load balancer ⚖️ 
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
 ✓ Joining more control-plane nodes 🎮 
 ✓ Joining worker nodes 🚜 
Set kubectl context to "kind-my-control-plane-ha-port-map"
You can now use your cluster with:

kubectl cluster-info --context kind-my-control-plane-ha-port-map

Not sure what to do next? 😅  Check out https://kind.sigs.k8s.io/docs/user/quick-start/
</pre>

- Check the containers created and the Port Binding as follows

<pre>
docker container ls
CONTAINER ID   IMAGE                                COMMAND                  CREATED              STATUS              PORTS                       NAMES
d1b76042d874   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute                               my-control-plane-ha-port-map-worker2
620d2f95a928   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute   0.0.0.0:8080->30000/tcp     my-control-plane-ha-port-map-worker
41c7b5b2f2db   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute   127.0.0.1:42799->6443/tcp   my-control-plane-ha-port-map-control-plane3
ccc9975495da   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute   127.0.0.1:33539->6443/tcp   my-control-plane-ha-port-map-control-plane
5f5e4e1537f1   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute   127.0.0.1:41847->6443/tcp   my-control-plane-ha-port-map-control-plane2
0eb4106f2838   kindest/haproxy:v20230606-42a2262b   "haproxy -W -db -f /…"   About a minute ago   Up About a minute   127.0.0.1:40783->6443/tcp   my-control-plane-ha-port-map-external-load-balancer
df750194f35d   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute                               my-control-plane-ha-port-map-worker3
</pre>

Look that there is one container where the Host Port is being redirected for Container Port: 8080->30000 

- Make a deployment

<pre>
vi /home/${USER}/Kubernetes/deployment-nginx.yaml
</pre>

<pre>
kubectl apply -f /home/${USER}/Kubernetes/deployment-nginx.yaml
</pre>

- Check deployment

<pre>
kubectl get svc
</pre>

[Example]

<code>

    kubectl get svc
    NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
    kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        4m15s
    nginx        NodePort    10.96.158.107   <none>        80:30000/TCP   11s

</code>

<pre>
kubectl get pods
</pre>

[Example]

<pre>
kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-6cfb64b7c5-92wjv   1/1     Running   0          109s
nginx-6cfb64b7c5-pfhn4   1/1     Running   0          109s
nginx-6cfb64b7c5-qcg25   1/1     Running   0          109s
nginx-6cfb64b7c5-xmq8n   1/1     Running   0          109s
</pre>

- Test deployment

<pre>
#Localhost
curl http://localhost:8080

#Remotely
curl http://192.168.0.24:8080
</pre>

###### KUBECTL: DEPLOYMENT (SAMPLE 5 - High Availability + Ports Mapping + Ingress Controller)

> NOTE: Reset the Kubernetes and KIND state
> <br />
> Reference: https://kind.sigs.k8s.io/docs/user/quick-start/#configuring-your-kind-cluster

> IMPORTANT: The Ingress Controller must be installed in the Control Plane ever, if try to 
> install in it worker node it will never fly 

- Get the configuration content and put in the yaml configuration file as follows

[Example]

Change the yaml file with something like below

<pre>
vi /home/${USER}/Kubernetes/ingress-controller-kind-example-config.yaml
</pre>

<pre>
apiVersion: kind.x-k8s.io/v1alpha4
kind: Cluster
nodes:
  - role: control-plane
    kubeadmConfigPatches:
      - |
        kind: InitConfiguration
        nodeRegistration:
          kubeletExtraArgs:
            node-labels: "ingress-ready=true"
    extraPortMappings:
      - containerPort: 80
        hostPort: 80
        protocol: TCP
      - containerPort: 443
        hostPort: 443
        protocol: TCP
  - role: control-plane
  - role: control-plane
  - role: worker
  - role: worker
  - role: worker
</pre>

- Create the clusters

<pre>
kind create cluster --name my-cluster-ingress-controller --config /home/${USER}/Kubernetes/ingress-controller-kind-example-config.yaml
</pre>

[Example]

<pre>
kind create cluster --name my-cluster-ingress-controller --config /home/${USER}/Kubernetes/ingress-controller-kind-example-config.yaml
Creating cluster "my-cluster-ingress-controller" ...
 ✓ Ensuring node image (kindest/node:v1.30.0) 🖼
 ✓ Preparing nodes 📦 📦 📦 📦 📦 📦  
 ✓ Configuring the external load balancer ⚖️ 
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
 ✓ Joining more control-plane nodes 🎮 
 ✓ Joining worker nodes 🚜 
Set kubectl context to "kind-my-cluster-ingress-controller"
You can now use your cluster with:

kubectl cluster-info --context kind-my-cluster-ingress-controller

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
</pre>

- Check nodes

<pre>
kubectl get nodes
</pre>

<code>

    NAME                                           STATUS   ROLES           AGE     VERSION
    my-cluster-ingress-controller-control-plane    Ready    control-plane   6m35s   v1.30.0
    my-cluster-ingress-controller-control-plane2   Ready    control-plane   6m18s   v1.30.0
    my-cluster-ingress-controller-control-plane3   Ready    control-plane   6m9s    v1.30.0
    my-cluster-ingress-controller-worker           Ready    <none>          5m59s   v1.30.0
    my-cluster-ingress-controller-worker2          Ready    <none>          5m59s   v1.30.0
    my-cluster-ingress-controller-worker3          Ready    <none>          6m      v1.30.0

</code>

- Check the containers created and the Port Binding as follows

<pre>
docker container ls
CONTAINER ID   IMAGE                                COMMAND                  CREATED         STATUS         PORTS                                                                 NAMES
806fbd25e747   kindest/haproxy:v20230606-42a2262b   "haproxy -W -db -f /…"   3 minutes ago   Up 3 minutes   127.0.0.1:38435->6443/tcp                                             my-cluster-ingress-controller-external-load-balancer
ceb47fb67026   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes   0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp, 127.0.0.1:46723->6443/tcp   my-cluster-ingress-controller-control-plane
7e05933ff941   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes                                                                         my-cluster-ingress-controller-worker2
716dfa1ac684   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes                                                                         my-cluster-ingress-controller-worker
32648cad7549   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes   127.0.0.1:46833->6443/tcp                                             my-cluster-ingress-controller-control-plane3
3e74001180cf   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes                                                                         my-cluster-ingress-controller-worker3
c44fc54c2399   kindest/node:v1.30.0                 "/usr/local/bin/entr…"   3 minutes ago   Up 3 minutes   127.0.0.1:38973->6443/tcp                                             my-cluster-ingress-controller-control-plane2
</pre>

Look that there is one container where the Host Port is being redirected for Container Port: 8080->30000

- Install NGINX Ingress Controller

<pre>
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
</pre>

[Example]

<pre>
namespace/ingress-nginx created
serviceaccount/ingress-nginx created
serviceaccount/ingress-nginx-admission created
role.rbac.authorization.k8s.io/ingress-nginx created
role.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrole.rbac.authorization.k8s.io/ingress-nginx created
clusterrole.rbac.authorization.k8s.io/ingress-nginx-admission created
rolebinding.rbac.authorization.k8s.io/ingress-nginx created
rolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
configmap/ingress-nginx-controller created
service/ingress-nginx-controller created
service/ingress-nginx-controller-admission created
deployment.apps/ingress-nginx-controller created
job.batch/ingress-nginx-admission-create created
job.batch/ingress-nginx-admission-patch created
ingressclass.networking.k8s.io/nginx created
validatingwebhookconfiguration.admissionregistration.k8s.io/ingress-nginx-admission created
</pre>

- Check Ingress Controller Namespace

<pre>
kubectl get all -n ingress-nginx
</pre>

<code>

    kubectl get all -n ingress-nginx
    NAME                                            READY   STATUS      RESTARTS   AGE
    pod/ingress-nginx-admission-create-g6wmg        0/1     Completed   0          92s
    pod/ingress-nginx-admission-patch-hvndp         0/1     Completed   0          92s
    pod/ingress-nginx-controller-56cbc5d9d4-j2lwv   1/1     Running     0          92s
    
    NAME                                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
    service/ingress-nginx-controller             NodePort    10.96.158.89    <none>        80:30140/TCP,443:32080/TCP   92s
    service/ingress-nginx-controller-admission   ClusterIP   10.96.193.172   <none>        443/TCP                      92s
    
    NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/ingress-nginx-controller   1/1     1            1           92s
    
    NAME                                                  DESIRED   CURRENT   READY   AGE
    replicaset.apps/ingress-nginx-controller-56cbc5d9d4   1         1         1       92s
    
    NAME                                       STATUS     COMPLETIONS   DURATION   AGE
    job.batch/ingress-nginx-admission-create   Complete   1/1           11s        92s
    job.batch/ingress-nginx-admission-patch    Complete   1/1           11s        92s

</code>

- Test the current setup

At this moment the Ingress Controller should be running and Nginx can be accessed vi http://localhost:80, but 
the things doesn't is done yet, so try to make a curl to check this status

<pre>
curl http://localhost:80

curl http://192.168.0.24:80
</pre>

Or try via Web Browser http://192.168.0.24:80

You should receive a Nginx page with the content 404 Not Found, because the configuration still be finished. 

[Example]

<code>

    <html>
    <head><title>404 Not Found</title></head>
    <body>
    <center><h1>404 Not Found</h1></center>
    <hr><center>nginx</center>
    </body>
    </html>

</code>

- Make a deployment

<pre>
vi /home/${USER}/Kubernetes/deployment-nginx.yaml
</pre>

<pre>
kubectl apply -f /home/${USER}/Kubernetes/deployment-nginx.yaml
</pre>

- Check deployment

<pre>
kubectl get svc
</pre>

[Example]

<code>

    kubectl get svc
    NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
    kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        4m15s
    nginx        NodePort    10.96.158.107   <none>        80:30000/TCP   11s

</code>

<pre>
kubectl get pods
</pre>

[Example]

<pre>
kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-6cfb64b7c5-92wjv   1/1     Running   0          109s
nginx-6cfb64b7c5-pfhn4   1/1     Running   0          109s
nginx-6cfb64b7c5-qcg25   1/1     Running   0          109s
nginx-6cfb64b7c5-xmq8n   1/1     Running   0          109s
</pre>

- Test deployment

<pre>
#Localhost
curl http://localhost:8080

#Remotely
curl http://192.168.0.24:8080
</pre>

- Create a Ingress Controller Config

<pre>
vi /home/${USER}/Kubernetes/deployment-ingress-nginx.yaml
</pre>

<pre>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-app-1
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-app-1
  template:
    metadata:
      labels:
        app: nginx-app-1
    spec:
      containers:
      - name: nginx-app-1
        image: nginx
        resources: {}
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-app-1
spec:
  selector:
    app: nginx-app-1
  ports:
    - port: 80
      targetPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-app-2
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-app-2
  template:
    metadata:
      labels:
        app: nginx-app-2
    spec:
      containers:
        - name: nginx-app-2
          image: nginx
          resources: {}
          ports:
          - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-app-2
spec:
  selector:
    app: nginx-app-2
  ports:
    - port: 80
      targetPort: 80
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-host
spec:
  ingressClassName: nginx
  rules:
    - host: app1.127.0.0.1.nip.io
      http:
        paths:
          - pathType: Prefix
            path: "/"
            backend:
              service:
                name: nginx-app-1
                port:
                  number: 80
    - host: app2.127.0.0.1.nip.io
      http:
        paths:
          - pathType: Prefix
            path: "/"
            backend:
              service:
                name: nginx-app-2
                port:
                  number: 80
</pre>

- Apply Ingress Controller Config

<pre>
kubectl apply -f /home/${USER}/Kubernetes/deployment-ingress-nginx.yaml
</pre>

[Example]

<pre>
kubectl apply -f /home/${USER}/Kubernetes/deployment-ingress-nginx.yaml
deployment.apps/nginx-app-1 created
service/nginx-app-1 created
deployment.apps/nginx-app-2 created
service/nginx-app-2 created
ingress.networking.k8s.io/ingress-host created
</pre>

- Check Ingress Controller Status

<pre>
kubectl get pods
NAME                           READY   STATUS    RESTARTS   AGE
nginx-app-1-55948c9775-58mlz   1/1     Running   0          38s
nginx-app-1-55948c9775-7ttqp   1/1     Running   0          38s
nginx-app-1-55948c9775-ktz7p   1/1     Running   0          38s
nginx-app-2-7c4c5db595-pj6zk   1/1     Running   0          38s
nginx-app-2-7c4c5db595-sx5h4   1/1     Running   0          38s
nginx-app-2-7c4c5db595-t28kz   1/1     Running   0          38s
</pre>

<code>

    kubectl get svc
    NAME          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
    kubernetes    ClusterIP   10.96.0.1      <none>        443/TCP   87m
    nginx-app-1   ClusterIP   10.96.173.25   <none>        80/TCP    96s
    nginx-app-2   ClusterIP   10.96.15.112   <none>        80/TCP    96s

</code>

<pre>
kubectl get ingress
NAME           CLASS   HOSTS                                         ADDRESS     PORTS   AGE
ingress-host   nginx   app1.127.0.0.1.nip.io,app2.127.0.0.1.nip.io   localhost   80      3m51s
</pre>

- Test Ingress Controller Config

> NOTE: In this case you can use the https://nip.io or the hosts file place in /etc/hosts (Linux) 
> to make the tests successfully 

<pre>
curl http://app1.127.0.0.1.nip.io
curl http://app2.127.0.0.1.nip.io
</pre>
