# Project 01: Setup Minikube

## Objective

In this project, you will set up a local Kubernetes cluster using Minikube and deploy a sample application to get familiar with basic Kubernetes operations.

## Steps

### 1. Install Minikube and kubectl

- **Minikube Installation:**
  Follow the [Minikube installation guide](https://minikube.sigs.k8s.io/docs/start/) to install Minikube on your local machine.

- **kubectl Installation:**
  Follow the [kubectl installation guide](https://kubernetes.io/docs/tasks/tools/) to install kubectl, the command-line tool for interacting with Kubernetes clusters.

### 2. Create a Cluster with Minikube

- Start Minikube and create a local Kubernetes cluster:
  ```bash
  minikube start
  ```
  This command will start a local Kubernetes cluster using a virtual machine or container.

### 3. Deploy a Sample Application

- Deploy a simple web server application:
  ```bash
  kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
  ```
  This command creates a deployment named `hello-minikube` using the `echoserver` image.

### 4. Expose the Application

- Expose the deployment as a service to make it accessible:
  ```bash
  kubectl expose deployment hello-minikube --type=NodePort --port=8080
  ```
  This command creates a service of type `NodePort` that maps port 8080 of the service to a port on the node.

### 5. Access the Application

- Get the URL to access the application:
  ```bash
  minikube service hello-minikube --url
  ```
  This command provides the URL where you can access the sample application running in your Minikube cluster.

### 6. Scale the Application

- Scale the deployment to run multiple replicas:
  ```bash
  kubectl scale deployment hello-minikube --replicas=3
  ```
  This command scales the deployment to 3 replicas.

### 7. Perform a Rolling Update

- Update the deployment with a new image:
  ```bash
  kubectl set image deployment/hello-minikube echoserver=k8s.gcr.io/echoserver:1.10
  ```
  This command performs a rolling update to the deployment by changing the container image to `echoserver:1.10`.

## Resources

- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)

## Files

- [deployment.yaml](resources/deployment.yaml) - YAML file for deploying the application.
- [service.yaml](resources/service.yaml) - YAML file for exposing the application as a service.


These files cover the basic setup for deploying a sample application in Minikube. The `README.md` provides instructions, while `deployment.yaml` and `service.yaml` define the Kubernetes resources needed for the deployment and service.