# K8S-ClusterSetup-Kubeadm
cluster setup on Ubuntu using kubeadm 

# what is kubeadm ?
Kubeadm is a tool built to provide kubeadm init and kubeadm join as best-practice "fast paths" for creating Kubernetes clusters.
kubeadm performs the actions necessary to get a minimum viable cluster up and running. By design, it cares only about bootstrapping, not about provisioning machine

**step 1: Install Docker**

sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker

**Install Kubernetes**

# install the apt-transport-https package, which will allow us to use http and https in Ubuntu’s repositories
 
sudo apt install apt-transport-https

# Install curl if not present
sudo apt install curl 

# Add the Kubernetes signing key  (use to verify the repositories from which k8s is installed)
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

# Adding k8s repo 
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

# kubeadm, kubectl and kubelet installation

sudo apt install kubeadm kubelet kubectl

# Make sure to disable swap memory (Kubernetes will refuse to function if your system is using swap memory)

sudo swapoff -a or 
sudo vi /etc/fstab (comment the swapfile line in this)

**Set hostnames**
sudo hostnamectl set-hostname kubernetes-master

# Initialize Kubernetes master server**
**
sudo kubeadm init

# The output from above also advises us to run several commands as a regular user to start using the Kubernetes cluster. Run those three commands on the master node

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
  
  # you can join the other nodes to cluster with the o/p command of kube init
  
  example of output command:  sudo kubeadm join 192.168.1.220:6443 --token 1exb8s.2t4k3b5syfc3jfmo --discovery-token-ca-cert-hash sha256:72ad481cee4918cf2314738419356c9a402fb609263adad48c13797d0cba2341


# depoly a pod Network (can use any cni plugin)
using flannel:
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/k8s-manifests/kube-flannel-rbac.yml

note: if you planed to run the cluster using Flannel CNI plugin make sure to pass **--pod-network-cidr=10.244.0.0/16** to kubeadm init. to work flannel correctly

