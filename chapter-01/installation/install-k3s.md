![Image](https://github.com/usmangt/learning-grafana/assets/69509548/3b921541-4376-4872-840f-d6dbbe8a51e6)

# Setting up K3s on a Linux Virtual Machine (minimal guide)

This guide is about how to setup and deploy a K3s Kubernetes cluster which will be running on Linux Virtual machine.

For a more detailed guide, please view my blog post by using this link.

<div align="center">
      <a href="https://www.youtube.com/watch?v=jocy_i6pyrs">
     <img 
      src="https://img.youtube.com/vi/jocy_i6pyrs/0.jpg" 
      alt="Everything Is AWESOME" 
      style="width:100%;">
      </a>
    </div>

# Introduction

[K3s](https://k3s.io/) is a lightweight Kubernetes distribution designed for resource-constrained environments, such as edge devices, IoT devices, and small clusters.  It is a highly optimized version of Kubernetes that aims to reduce the memory and CPU footprint while maintaining full compatibility with the Kubernetes API.

It was originally developed by [Rancher(Suse)](https://www.rancher.com/)

![image](https://github.com/usmangt/learning-grafana/assets/69509548/9e859578-889d-4b6c-8073-b5ab1ce18c40)


K3s is a now CNCF [Sandbox Project](https://www.cncf.io/).

![image](https://github.com/usmangt/learning-grafana/assets/69509548/030f2118-6bb1-48ef-a6bf-c18c29094deb)


# Installation

## Step 1 - Install the required packages

Install the following packages which are required for the K3s installation.

```bash
dnf -y install setroubleshoot-server curl tar
```

Now run the K3s installer script:

```bash
curl -sfL https://get.k3s.io | sh
```
## Step 2 - Verifying K3s Service

```bash
systemctl status k3s
```

# Working on K3s

Let's test out our K3s deployment using the basic commands:

```bash
kubectl get pods --all-namespaces
```
It will show all the Pods.

This means that our K3s cluster is running fine-
