# Setting up k3s on CentOS Virtual Machine

This guide is about how to setup and deploy a K3s Kubernetes cluster which will be running on a Virtual machine.


# Introduction

[k3s](https://k3s.io/) is a lightweight Kubernetes distribution designed for resource-constrained environments, such as edge devices, IoT devices, and small clusters.  It is a highly optimized version of Kubernetes that aims to reduce the memory and CPU footprint while maintaining full compatibility with the Kubernetes API.

It was originally developed by [Rancher(Suse)](https://www.rancher.com/)

![image](https://github.com/usmangt/learning-grafana/assets/69509548/9e859578-889d-4b6c-8073-b5ab1ce18c40)


k3s is a now CNCF [Sandbox Project](https://www.cncf.io/)

![image](https://github.com/usmangt/learning-grafana/assets/69509548/030f2118-6bb1-48ef-a6bf-c18c29094deb)

## Advantages

k3s offers several advantages over other Kubernetes distributions like Minikube and kinD etc:

- **Lightweight and Resource-Efficient**: k3s is designed to be lightweight and optimized for resource-constrained environments. It has a smaller memory and CPU footprint compared to other Kubernetes distributions, making it suitable for running on edge devices, IoT devices, and small clusters.

- **Easy Installation and Management**: k3s provides a simplified installation process, making it easier to set up and manage Kubernetes clusters. It has minimal dependencies and can be installed with a single binary, reducing the complexity and time required for installation.

- **High Availability (HA) Support**: k3s supports high availability out of the box, allowing you to create highly resilient Kubernetes clusters. It uses built-in features like embedded etcd and automatic leader election to ensure cluster availability even in the event of node failures.

- **Integrated Add-Ons**: k3s includes several integrated add-ons that enhance the functionality of Kubernetes. For example, it includes kube-vip for load balancing and MetalLB for external access to services. These add-ons simplify the configuration and management of common Kubernetes tasks.

- **Security and Simplicity**: k3s prioritizes security and simplicity. It removes or replaces certain components that are not essential for most use cases, reducing the attack surface and making the cluster more secure. Additionally, k3s provides a simplified and opinionated approach to Kubernetes, making it easier for users to adopt and manage.



# Installation

## Requirements

These are the Minimal system requirements to setup and deploy a single node k3s on any machine.

1- Architecture

k3s is available for the following architectures:

- x86_64
- armhf
- arm64/aarch64
- s390x

2- Operating Systems

kK3s is expected to work on most modern Linux systems.


For detailed installation, refer to the official [docs](https://docs.k3s.io/).

## For CentOS

```bash
dnf -y upgrade
```

reboot if necessary

```bash
dnf -y install epel-release
dnf -y update
```
Now, install the required packages:

```bash
dnf -y install setroubleshoot-server curl lsof wget tar epel-release vim
```

For **CentOS 7**, just replace the above `dnf` command with `yum`.


```bash
systemctl stop firewalld
systemctl disable firewalls
```


```bash
vim /etc/selinux/config 
```

Set,

`SELINUX=disabled`

Set a valid `hostname` inside the file `/etc/hosts`

reboot it again

Now run the K3s installer script:

```bash
curl -sfL https://get.k3s.io | sh
```

Lets review the service in detail
```bash
cat /etc/systemd/system/k3s.service
```

Also, lets see the current status:

```bash
systemctl status k3s
```

```bash
kubectl get nodes
```
# all pods in running state? fine!
```bash
kubectl get pods --all-namespaces
```



# Test K3s
Here, you will test your K3s cluster with a simple NGINX website deployment.


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

Deploy the NGINX website on your K3s cluster:

```bash
kubectl apply -f ./nginx.yaml
```

The expected output is similar to:

deployment.apps/nginx created
service/nginx created

Verify that the pods are running:

```bash
kubectl get pods
```

Verify that your deployment is ready:

```bash
kubectl get deployments
```

The expected output is similar to:

```bash
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           57s
```

Verify that the load balancer service is running:

```bash
kubectl get services nginx
```
The expected output is similar to:

```bash
NAME       TYPE           CLUSTER-IP    EXTERNAL-IP       PORT(S)          AGE
nginx      LoadBalancer   10.0.0.89     192.0.2.1         8081:31809/TCP   33m
```

In a web browser navigation bar, type the IP address listed under `EXTERNAL_IP` from your output and append the port number:`8081` to reach the default NGINX welcome page.

Delete your test NGINX deployment:

```bash
kubectl delete -f ./nginx.yaml
```
