# Project 02: Create a Kubernetes Cluster with kubeadm

## Objective

In this project, you will create a Kubernetes cluster using kubeadm. This will involve setting up the control plane node and worker nodes, configuring network settings, and ensuring the cluster is functioning correctly.

## Steps

### 1. Prepare the Environment

Before you begin, make sure you have a clean environment. You can use virtual machines, cloud instances, or physical machines. Ensure that each machine has the following minimum requirements:

- 2 GB or more of RAM per machine
- 2 CPUs or more
- Full network connectivity between all machines in the cluster
- Unique hostname, MAC address, and product_uuid for every node

### 2. Install kubeadm, kubelet, and kubectl

Follow the official [Installing kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/) guide to install kubeadm, kubelet, and kubectl on each node.

#### Commands

1. **Update the package index and install packages needed to use the Kubernetes apt repository**:

    ```bash
    sudo apt-get update
    sudo apt-get install -y apt-transport-https ca-certificates curl
    ```

2. **Download the Google Cloud public signing key**:

    ```bash
    sudo curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    ```

3. **Add the Kubernetes apt repository**:

    ```bash
    sudo bash -c 'cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
    deb https://apt.kubernetes.io/ kubernetes-xenial main
    EOF'
    ```

4. **Update apt package index, install kubelet, kubeadm, and kubectl, and pin their version**:

    ```bash
    sudo apt-get update
    sudo apt-get install -y kubelet kubeadm kubectl
    sudo apt-mark hold kubelet kubeadm kubectl
    ```

### 3. Initialize the Control Plane Node

On the control plane node, run the following command to initialize the cluster:

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

This command will set up the control plane components and create a new cluster. The `--pod-network-cidr` option specifies the CIDR block for the pod network.

### 4. Set Up kubeconfig for the Control Plane Node

To start using your cluster, you need to set up the kubeconfig file:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### 5. Install a Pod Network Addon

Install a pod network addon so that your pods can communicate with each other. Here is an example using the Calico network plugin:

```bash
kubectl apply -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml
```

### 6. Join Worker Nodes to the Cluster

On each worker node, run the `kubeadm join` command that was output when you initialized the control plane node. If you didn't save the command, you can generate a new token on the control plane node:

```bash
kubeadm token create --print-join-command
```

Run the output command on each worker node to join them to the cluster.

### 7. Verify the Cluster

Check the status of the nodes to ensure they are all part of the cluster:

```bash
kubectl get nodes
```

You should see a list of all nodes in the cluster with a STATUS of "Ready".

### 8. Deploy a Sample Application

Deploy a simple web server application to verify your cluster is working:

```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=NodePort
```

Get the NodePort and access the application via a web browser or curl command:

```bash
kubectl get services
```

## Resources

- [Installing kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- [Creating a cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)

---
