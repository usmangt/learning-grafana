# Grafana Installation on Docker (Linux)

## Part 2 - How to Use Grafana with Docker


## 1 - System Requriements

- Docker Server:
  
    Ensure the docker server and other dependent services such as **contained** are up and running.

- Port 3306 open
  
    Make sure port `3306` is open.


## 2 - Using Docker Hub

We will use the Docker Hub (https://hub.docker.com/) to find the correct Grafana image.

>📌 **NOTE:** You need to create an account which is free.

Also, we can do the same by using the command line:

```sh
docker search grafana-oss
```

## 3 Grafana default Paths (only when using Docker)

Following are the default paths for the Grafana image.

|  Setting      	|  Default value |
|-------------------|---------------|
|GF_PATHS_CONFIG |	`/etc/grafana/grafana.ini` |
|GF_PATHS_DATA |	`/var/lib/grafana` |
|GF_PATHS_HOME |	`/usr/share/grafana` |
|GF_PATHS_LOGS	| `/var/log/grafana` |
|GF_PATHS_PLUGINS | `/var/lib/grafana/plugins` |
|GF_PATHS_PROVISIONING |	`/etc/grafana/provisioning` |

## 4 - Using the docker run command

### 1. Creating persistent storage (recommended)
It is recommended to have persistent storage because, without it, all data will be lost once the container is shut down. Use this method when you want the Docker service to manage the storage volume fully.

  1. To use persistent storage, complete the following steps:

     Create a Grafana Docker volume grafana-storage by running the following commands:


      ```sh
      docker volume create grafana-storage
      ```

  2. verify that the volume was created correctly, you should see a JSON output.

     ```sh
     docker volume inspect grafana-storage
     ```

### 2. Deploying Grafana using the docker run command

This is the command syntax:

```sh
docker run -d -p 3000:3000 --name grafana grafana/grafana-oss:<version number>
```

Run the above command with the desired version i.e. to run the latest version

```sh
docker run -d -p 3000:3000 --name grafana  --volume grafana-storage:/var/lib/grafana grafana/grafana-oss:latest
```

>📌 **NOTE:** You can find the version number from the Docker hub website.


### 3. Verify the status and login.

1. Use the `docker ps` command to see if it is working correctly:
   ```sh
   docker ps
   ```

2. Login to the WebUI by using the **admin** as both username and password


### 4. Stop the container

Use the following command to stop the container:

```sh
docker stop grafana
```



## 5 Using the docker compose command

1. First we verify if the docker compose plugin is availalbe by running the command:

    ```sh
    docker compose version
    ```

>📌 **NOTE:** Incase if it is not available on your machine then you can follow this tutorial [link](https://github.com/usmangt/learning-grafana/blob/master/chapter-01/installation/install-on-docker-part-1.md#4--install-docker-engine), where I have explained as how to install it.


2. Now we create a file named `docker-compose.yaml`

    ```sh
    vim docker-compose.yaml
    ```

3. Add the following YAML code into it.

    ```yaml
    version: '3.8'
    services:
      grafana:
        image: grafana/grafana-enterprise
        container_name: grafana
        restart: unless-stopped
        ports:
          - '3000:3000'
        volumes:
          - grafana_data:/var/lib/grafana
    volumes:
      grafana_data: {}
    ```

Save the changes in the file.


4. Now run the `docker compose` command:

    ```sh
    docker compose up -d
    ```

5. Verify the status using `docker ps` command:

    ```sh
    docker ps
    ```   

6. Login to the WebUI by using the **admin** as both username and password


### 4. Stop the container

Use the following command to stop the container:

```sh
docker compose down
```
Verify if it is correctly shut down by using the docker ps command:

```sh
docker ps
```

## 5 Troubleshooting

### 1. Deleting the previously exited container image

To delete a previously used image even after stopping the container, run the following command to the list only of the exited container:

```sh
docker container ls -a --filter status=exited --filter status=created 
```

Once, you know the image ID, then remove that container by running the command:

```sh
docker rm <IMAGE NAME>
```


### 2. Checking logs

Use the docker logs command to investigate the logs:


```sh
docker logs <CONTAINER-NAME>
```






<!---
//comment
[root@centos7 ~]# docker inspect grafana/grafana - for inspecting path

-->




