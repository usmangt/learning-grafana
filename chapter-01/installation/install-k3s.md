# Installing K3s on CentOS Virtual Machine


## For CentOS 8

dnf -y upgrade

reboot if necessary

dnf -y install epel-release
dnf -y update

dnf -y install setroubleshoot-server curl lsof wget tar epel-release vim

For CentOS 7, just replace the above dnf command with yum


[root@k3s-demo ~]# systemctl stop firewalld
[root@k3s-demo ~]# systemctl disable firewalls

[root@k3s-demo ~]# vim /etc/selinux/config 

Set,

SELINUX=disabled

Set hostname /etc/hosts

reboot it again

curl -sfL https://get.k3s.io | sh

cat /etc/systemd/system/k3s.service
systemctl status k3s


kubectl get nodes
# all pods in running state? fine!
kubectl get pods --all-namespaces




# Test K3s
Here, you will test your K3s cluster with a simple NGINX website deployment.


File: nginx.yaml

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



Save and close the nginx.yaml file.

Deploy the NGINX website on your K3s cluster:

kubectl apply -f ./nginx.yaml

The expected output is similar to:

deployment.apps/nginx created
service/nginx created
Verify that the pods are running:

kubectl get pods


Verify that your deployment is ready:

kubectl get deployments

The expected output is similar to:

NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           57s
Verify that the load balancer service is running:

kubectl get services nginx

The expected output is similar to:

NAME       TYPE           CLUSTER-IP    EXTERNAL-IP       PORT(S)          AGE
nginx      LoadBalancer   10.0.0.89     192.0.2.1         8081:31809/TCP   33m

In a web browser navigation bar, type the IP address listed under EXTERNAL_IP from your output and append the port number :8081 to reach the default NGINX welcome page.

Delete your test NGINX deployment:

kubectl delete -f ./nginx.yaml

