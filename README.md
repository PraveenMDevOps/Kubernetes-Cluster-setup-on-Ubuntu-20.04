# Kubernetes-Cluster-setup-on-Ubuntu-20.04

## What is Kubernetes?

Kubernetes, often abbreviated as "k8s" (because it has 8 letters between the 'k' and the 's'), is an open-source container orchestration platform. Originally developed by Google and now maintained by the Cloud Native Computing Foundation (CNCF), Kubernetes automates the deployment, scaling, and management of containerized applications.

Containers have become a popular way to package, distribute, and run applications because they provide a lightweight, portable, and consistent environment across different computing environments. However, managing a large number of containers across multiple machines can be complex and challenging. This is where Kubernetes comes in.

----
###### Benefits of Kubernetes
1. Kubernetes is good at managing containers across diverse nodes;
2. Kubernetes is compatible with many public and private cloud infrastructures;
3. Kubernetes allows you to automatically scale your application when it needs more resources to run successfully;
4. Kubernetes self-healing feature automatically restarts failing containers and replaces unhealthy containers.

----
###### Prerequisites
To install Kubernetes on your Ubuntu machine, make sure it meets the following requirements:

     1. 3 Ubuntu 20.04 machines
     2. 2 CPUs
     3. At least 2GB of RAM
     4. At least 2 GB of Disk Space
If your machine meets the above requirements, you are ready to follow this tutorial. Let’s start with the step-by-step process on how to install Kubernetes on Ubuntu 20.04.


#### Install Kubernetes on Ubuntu: Step-by-step process

Overall, installing Kubernetes on Ubuntu involves steps such as:

      1. Disabling swap
      2. Install Docker Engine
      3. Install Support Packages
      4. Retrieve the key for the Kubernetes repo and add it to your key manager
      5. Add the kubernetes repo to your system
      6. Install the three pieces you’ll need, kubeadm, kubelet, and kubectl
      7. Create the actual cluster
      8. Install the Calico network plugin
      9. Get Cluster Nodes

----
###### Disable swap
Swap can be disabled on all nodes by using the below commands.

~~~
swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
~~~

###### Install Docker Engine
To install Docker Engine on all nodes, execute the following command on each node.

~~~
apt-get install -y docker.io
~~~

###### Install Support Packages
Execute the following command on each node.

~~~
apt-get install -y apt-transport-https curl
~~~

###### Retrieve the key for the Kubernetes repo and add it to your key manager
Execute the following command on each node.

~~~
mkdir -p /etc/apt/keyrings/
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
~~~
###### Add the kubernetes repo to your system
To add the K8s repo, execute the following command on each node.

~~~
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list
~~~

###### Install the three pieces you’ll need, kubeadm, kubelet, and kubectl
Execute the following command on each node.

~~~
apt-get update
apt-get install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
~~~

###### Create the actual cluster
Execute the following command on the Control Plane only.

~~~
kubeadm init --pod-network-cidr=192.168.0.0/16
~~~
You will get Joint Tocken from the console and Its looks like below image.

![Screenshot from 2023-12-20 14-43-59](https://github.com/PraveenMDevOps/Kubernetes-Cluster-setup-on-Ubuntu-20.04/blob/main/k8s_cluster_done.png)

PLease run the below commands and the Joint tocken in the Worker-Node servers.
~~~
export KUBECONFIG=/etc/kubernetes/admin.conf
source ~/.bashrc
~~~

###### Install the Calico network plugin
Execute the below command on the Control Plane server only.

~~~
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
~~~

###### Get Cluster Nodes
Then run the below command in the Master node and you will get the inforamtion about the nodes and It will active state.

~~~
kubectl get node 
~~~

![Screenshot from 2023-12-20 14-49-39](https://github.com/PraveenMDevOps/Kubernetes-Cluster-setup-on-Ubuntu-20.04/blob/main/get_nodes.png)

Here you can see my master node and worker nodes are activate state. 

Also, please use the below command to retrieve information about pods running in the kube-system namespace in the Kubernetes cluster to verify the cluster is working fine.

![Screenshot from 2023-12-20 14-49-40](https://github.com/PraveenMDevOps/Kubernetes-Cluster-setup-on-Ubuntu-20.04/blob/main/get_pods.png)


###### 
### Conclusion
This guide covered how to install Kubernetes on Ubuntu 20.04 and set up a cluster.

---

