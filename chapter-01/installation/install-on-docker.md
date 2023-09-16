# Grafana Installation on Docker

## Part 1 - Installing Docker

### 1 - System Requriements

- Operating System:
Docker Engine supports a wide range of Linux distributions such as Ubuntu, Debian, CentOS, Fedora, and RHEL etc. Please make sure your Linux distribution is officially supported by Docker by using the following link.
- Architecture:
Docker supports x86_64 (64-bit), armhf (32-bit ARM), and arm64 (64-bit ARM) architectures. Ensure that your system’s architecture is compatible.
- Kernel-Version:
Docker requires a Linux kernel version of 3.10 or higher. You can check your kernel version by running the command:
`uname -r`
- CPU:
Docker typically works well with modern CPUs. However, virtualization extensions (e.g., Intel VT-x or AMD-V) need to be enabled in the BIOS/UEFI firmware for optimal performance.
- RAM:
At least a minimum of 2 GB of RAM for the host system. However, the more the better depending on your use case and the number of containers you plan to run.
- Disk Space:
The recommended minimum disk space is 20 GB. However, the more the better, depending on your plan to work with large images or data volumes.

### 2 - Installing Docker Engine on CentOS

#### Prerequisites
##### OS requirements
To install Docker Engine, you need a maintained version of one of the following CentOS versions:

- CentOS 7
- CentOS 8 (stream)
- CentOS 9 (stream)

NOTE: The centos-extras repository must be enabled. This repository is enabled by default, but if you have disabled it, you need to re-enable it


## Part 2 - Install Grafana using Docker
Coming Soon
