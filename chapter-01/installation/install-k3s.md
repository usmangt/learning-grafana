![Image](https://github.com/usmangt/learning-grafana/assets/69509548/3b921541-4376-4872-840f-d6dbbe8a51e6)

# Setting up K3s on a Linux Virtual Machine

This guide is about how to setup and deploy a K3s Kubernetes cluster which will be running on Linux Virtual machine.


# Introduction

[K3s](https://k3s.io/) is a lightweight Kubernetes distribution designed for resource-constrained environments, such as edge devices, IoT devices, and small clusters.  It is a highly optimized version of Kubernetes that aims to reduce the memory and CPU footprint while maintaining full compatibility with the Kubernetes API.

It was originally developed by [Rancher(Suse)](https://www.rancher.com/)

![image](https://github.com/usmangt/learning-grafana/assets/69509548/9e859578-889d-4b6c-8073-b5ab1ce18c40)


K3s is a now CNCF [Sandbox Project](https://www.cncf.io/).

![image](https://github.com/usmangt/learning-grafana/assets/69509548/030f2118-6bb1-48ef-a6bf-c18c29094deb)

## Advantages

K3s offers several advantages over other Kubernetes distributions like Minikube and kinD etc:

- **Lightweight and Resource-Efficient**: k3s is designed to be lightweight and optimized for resource-constrained environments. It has a smaller memory and CPU footprint compared to other Kubernetes distributions, making it suitable for running on edge devices, IoT devices, and small clusters.

- **Easy Installation and Management**: k3s provides a simplified installation process, making it easier to set up and manage Kubernetes clusters. It has minimal dependencies and can be installed with a single binary, reducing the complexity and time required for installation.

- **High Availability (HA) Support**: k3s supports high availability out of the box, allowing you to create highly resilient Kubernetes clusters. It uses built-in features like embedded etcd and automatic leader election to ensure cluster availability even in the event of node failures.

- **Integrated Add-Ons**: k3s includes several integrated add-ons that enhance the functionality of Kubernetes. For example, it includes kube-vip for load balancing and MetalLB for external access to services. These add-ons simplify the configuration and management of common Kubernetes tasks.

- **Security and Simplicity**: k3s prioritizes security and simplicity. It removes or replaces certain components that are not essential for most use cases, reducing the attack surface and making the cluster more secure. Additionally, k3s provides a simplified and opinionated approach to Kubernetes, making it easier for users to adopt and manage.

# Minimum Requirements

These are the Minimal system requirements to setup and deploy a single node k3s on any machine.

**1. Architecture**

k3s is available for the following architectures:

- x86_64
- armhf
- arm64/aarch64
- s390x

**2. Operating Systems**

K3s is expected to work on most modern Linux systems.

**3. Minimum Hardware requirements**

Minimum recommendations are outlined here.

|Spec           |Minimum	  | Recommended |             
|---------------|-----------|-------------|
|CPU            |1 core            |2 cores |
|RAM            |512 MB            |1 GB |

**4. Networking**

The K3s server needs port `6443` to be accessible by all nodes.

For detailed installation, refer to the official [docs](https://docs.k3s.io/).

# Installation

### Pre-requisite

> 📌 You will need to disable the SWAP Disk space on your machine. That means, on a very basic level we only need a Boot and the Root partition.
>
> You can check my [video](https://www.youtube.com/watch?v=APZ5FDpxNVY) where I explained on how to create custom partitions while installing Linux.

## Step 1 - Setting up the Linux OS

We will be using CentOS as our primary O.S. for this guide. You can use any other Linux distribution of your choice as the steps will be the same.

For **CentOS** first update the system using the command:

```bash
dnf -y upgrade
```
For **CentOS 7**, just replace the above `dnf` command with `yum`.

📌 Reboot the machine if required. 

After that, we install the `epel-release` package to get more up-to-date packages.

```bash
dnf -y install epel-release
dnf -y update
```

## Step 2 - Disable Firewall and SELinux

Since we are not deploying our machine in a real production environment, therefore we can simply disable the Linux Firewall and SELinux services to avoid issues that may arrive later.

First, let's disable the Firewall service.
  
```bash
systemctl stop firewalld
systemctl disable firewalls
```

Now, let's disable the SELinux configuration.

Open the following file `/etc/selinux/config` in your favorite editor:

Set the value for `SELINUX`

`SELINUX=disabled`

Save and exit

## Step 3 - Install the required packages

Set a valid `hostname` inside the file `/etc/hosts`

Now reboot the machine so that the above changes take effect.

## Step 4 - Install the required packages

Install the following packages which are required for the K3s installation.

```bash
dnf -y install setroubleshoot-server curl lsof wget tar epel-release vim
```

Now run the K3s installer script:

```bash
curl -sfL https://get.k3s.io | sh
```
## Step 5 - Verifying K3s Service

Let's review the service in detail:

```bash
cat /etc/systemd/system/k3s.service
```

Also, let's see the current status of the service:

```bash
systemctl status k3s
```

# Working on K3s

Let's test out our K3s deployment using the basic commands:

```bash
kubectl get pods --all-namespaces
```
It will show all the Pods.

This means that our K3s cluster is running fine


# Testing K3s
Let's deploy a Nginx server on our K3s cluster.

## Step 1 - Creating the deployment file

File: `nginx.yaml`

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
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
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
    - protocol: TCP
      port: 8081
      targetPort: 80
  selector:
    app: nginx
  type: LoadBalancer
```

Save and close the `nginx.yaml` file.

## Step 2 - Deploying the file

Deploy the Nginx on your K3s cluster:

```bash
kubectl apply -f nginx.yaml
```

> The expected output is similar to:
> ```bash
> deployment.apps/nginx created
> service/nginx created
> ```

Also, verify that the pods are running:

```bash
kubectl get pods
```

Check the deployment if it is ready:

```bash
kubectl get deployments
```

> The expected output is similar to:
> ```bash
> NAME    READY   UP-TO-DATE   AVAILABLE   AGE
> nginx   1/1     1            1           57s
> ```

## Step 3 - Accessing the Service

Verify that the load balancer service is running:

```bash
kubectl get services nginx
```
> The expected output is similar to:
> ```bash
> NAME       TYPE           CLUSTER-IP    EXTERNAL-IP       PORT(S)          AGE
> nginx      LoadBalancer   10.0.0.89     192.0.2.1         8081:31809/TCP   33m
> ```

In a web browser navigation bar, type the IP address listed under `EXTERNAL_IP` from your output and append the port number:`8081` to reach the default NGINX welcome page.

## Step 4 - Deleting the Deployment

To delete your test Nginx deployment, run the command:

```bash
kubectl delete -f nginx.yaml
```
