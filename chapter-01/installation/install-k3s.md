# Installing K3s on CentOS Virtual Machine


## For CentOS 8

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
