# Grafana Installation on Docker (Linux)

<!--
**Table of content:**
 - [Part 1 - Installing Docker](## Part 1)
 - [First Item](#item-two)
 - [Second Item](#item-three)

-->

## Part 1 - Installing Docker

### 1 - System Requriements

- Operating System:

	Docker Engine supports a wide range of Linux distributions such as Ubuntu, Debian, CentOS, Fedora, RHEL, etc. Please make sure your Linux distribution is officially supported by Docker by using the following [link](https://docs.docker.com/engine/install/#supported-platforms).  
	You can find the distribution detail by using the command:
	```bash
	cat /etc/os-release
	```
- Architecture:

	Docker supports `x86_64 (64-bit)`, `armhf (32-bit ARM)`, and `arm64 (64-bit ARM)` architectures. Ensure that your system’s architecture is compatible.
	You can check your kernel version by running the command:
	```bash
	lscpu
	```

- Kernel-Version:
 
	Docker requires a Linux kernel version of `3.10` or higher.
	You can check your kernel version by running the command:
	```bash
	uname -r
	```
- CPU:
	Docker typically works well with modern CPUs. However, virtualization extensions (e.g., **Intel VT-x** or **AMD-V**) need to be enabled in the BIOS/UEFI firmware for optimal performance. 
	You can check your kernel version by running the command:
	```bash
	lscpu
	```

- RAM:
	At least a minimum of **2 GB of RAM** for the host system. However, the more the better depending on your use case and the number of containers you plan to run.
	You can find the total free/used memory information by running the command:
	```bash
	free -h
	```
- Disk Space:
	The recommended minimum disk space is **20 GB**. However, the more the better, depending on your plan to work with large images or data volumes.
	You can check disk space by using the command:
	```bash
	df -h
	```

### 2 - Installing Docker Engine on CentOS

#### Prerequisites
We will be using the Linux CentOS distribution to install Docker. If you are using any other Linux Distribution e.g. Debian, SLES, etc. then please follow this [link](https://docs.docker.com/engine/install/#server) to their respective documentation.

#### 1- OS requirements

To install Docker Engine, you need a maintained version of one of the following CentOS versions:


|  Version      	|  Command to use for installing packages |
|-------------------|----------------------------|
|CentOS 7          | `yum`            			 |
|CentOS 8 (stream) | `dnf`            			 |
|CentOS 9 (stream) | `dnf`            			 |

> 📌 **NOTE:** The `centos-extras` repository must be enabled. This repository is enabled by default, but if you have disabled it, you need to re-enable it

#### 2- Uninstall any old/obsolete/unstable versions 

Older versions of Docker went by the names of `docker` or `docker-engine`. Uninstall any such older versions before attempting to install a new version, along with associated dependencies. To do this, login as a root user (or can use the `sudo` command) and then run:

```bash
yum remove docker \
	docker-client \
	docker-client-latest \
	docker-common \
	docker-latest \
	docker-latest-logrotate \
	docker-logrotate \
	docker-engine
```

#### 3- Setup Docker Engine repository

Install the `yum-utils` package (which provides the `yum-config-manager` utility) and set up the repository.

```bash
yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

#### 4- Install Docker Engine

Now install the latest version of the Docker Engine, containerd, and Docker Compose packages:

```bash
yum install docker-ce \
	docker-ce-cli \
	containerd.io \
	docker-buildx-plugin \
	docker-compose-plugin
```

>📌 **NOTE:** If prompted to accept the GPG key, verify that the fingerprint matches `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`, and if so, accept it.


### 3 - Configuring up the Docker service

#### 1- Starting up the Docker services

1. To start the Docker service, you will need to start both the docker and contained server by using the command:

```bash
systemctl start docker
systemctl start containerd
```

2. Verify if both of the services are running correctly by running the command:

```bash
systemctl status docker
systemctl status containerd
```

If there are no errors then, it means that the service is ready.

#### 2- Testing up the service

Now, test and verify the docker service by pulling up a test container image:

```bash
docker run hello-world
```
It will print a confirmation message and then exit. This means that the Docker is working properly.

#### 3- Enabling service on boot

It will be a good idea to enable the Docker service and other needed dependencies to get loaded during the boot process. To do that run the following commands:

```bash
systemctl enable docker.service
systemctl enable containerd.service
```

### 4 - Uninstall Docker

If you may need to uninstall Docker then follow the following steps:


1. Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages:

```bash
yum remove docker-ce \
  docker-ce-cli \
  containerd.io \
  docker-buildx-plugin \
  docker-compose-plugin \
  docker-ce-rootless-extras \
```

2. Remove local data

To remove Images, containers, volumes, or custom configuration files on your host that aren't automatically removed, run the following commands:

```bash
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

















## Part 2 - Install Grafana using Docker
Coming Soon
