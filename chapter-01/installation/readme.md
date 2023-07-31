# Grafana Installation

## 1. What is Grafana?

- It is a tool to monitor and visualize data.

- It is **totally free** (open source) and cross-platform supported

- It does not require you to ingest data to a backend database as instead, Grafana only needs to connect to your Data source to extract and display the data.

- Very widely used for application and system Obersvability e.g. Kubernetes, cloud, etc.

- Can easily create Dashboards and share them as required.

- Provide powerful features to transform and filter the data to a meaningful statistics and other features such as Alerting, Tracing, etc.

### 2. What are the different editions of Grafana?

| Grafana OSS | Grafana Enterprise | Grafana Cloud |
| ----------- | ------------------ | ------------- |
| For users who prefer to set up, administer, and maintain their own installation | For organizations that have specific privacy or security requirements and need a self-managed environment |  Managed and administered by Grafana Labs with free and paid options for individuals, teams, and large enterprises |
| No Support SLA | Support SLA included  | Support SLA included |
| Basic (core features) | Enterprise data source plugins and built-in collaboration features | Enterprise + Cloud specific features |

For more information, refer here [Cloud vs Enterprise](https://grafana.com/grafana/deployment-options/).

# System Requirements

## 1. Hardware requirements

Grafana requires the minimum system resources:

    Minimum recommended memory: 255 MB
    Minimum recommended CPU: 1

> **Note:**  Some features might require more memory or CPUs, including Server side rendering of images, Alerting, Data source proxy.

## 2. Database requirements

| Name | Version |
| ---- | ------- |
| [Sqlite](https://www.sqlite.org/index.html) (**default**) | 3.x |
| [MySQL](https://www.mysql.com/support/supportedplatforms/database.html) | 5.7.x or higher | 
| [PostgreSQL](https://www.postgresql.org/support/versioning/) | 10.x or higher |

## 3. Browser requirements

Always use the latest and stable versions of the following:

- Chrome/Chromium
- Firefox
- Safari
- Microsoft Edge

> **Note:**  JavaScript must be enabled in your browser. Running Grafana without JavaScript enabled in the browser is not supported.

# Installation

## 1. Update the machine

For e.g.

- On **RPM** based distro. `dnf update` (if `dnf` command is not available then use `yum update`)

- On **Debian** based disto. `sudo apt-get update`

## 2. Choosing the correct version release

Grafana OSS is avaialbe in 2 types of releases:

     1. OSS Stable (stable-branch)
     2. Nightly Builds (dev-branch)

Use the OSS Stable release as it is more stable and contains all the information in the release notes.

## 3. Installation via the official repository

### Getting the GPG Key

1. Download the GPG key:

    ```bash
    wget -q -O gpg.key https://rpm.grafana.com/gpg.key
    ```

1. Import the key:

    ```bash
    rpm --import gpg.key
    ```

### Creating the repository on the machine

1. Create a new file e.g. `grafana.repo` in the `(/etc/yum.repos.d/)` directory :

    ```bash
    touch /etc/yum.repos.d/grafana.repo
    ```

2. Paste the following lines to it:

    ```bash
    [grafana]
    name=grafana
    baseurl=https://rpm.grafana.com
    repo_gpgcheck=1
    enabled=1
    gpgcheck=1
    gpgkey=https://rpm.grafana.com/gpg.key
    sslverify=1
    sslcacert=/etc/pki/tls/certs/ca-bundle.crt

    # to avoid installing any beta release add this line
    exclude=*beta*
    ```

### Download and install Grafana

Run the following command to download and install the Grafana package:

```bash
yum install grafana
```

> **Note:**  If you are using RHEL/CentOS 8 or higher then use the `dnf` command instead of `yum`.

# Configuring System services

1. To start the Grafana service so that be able to use it, run the command:

    ```bash
    systemctl start grafana-server
    ```

1. Check the status if it is active or not via command:

    ```bash
    systemctl status grafana-server
    ```

    The output will look similar to this:

    ```bash
    ● grafana-server.service - Grafana instance
    Loaded: loaded (/usr/lib/systemd/system/grafana-server.service; disabled; vendor preset: disabled)
    Active: active (running) since Fri 2023-07-28 11:38:15 CEST; 6s ago
    ```

    Here it does states that the service is `active (running)` which confirms that we can now access Grafana.

1. Enable the Grafana service to load automatically when the system boots:

   ```bash
    systemctl enable grafana-server
    ```

# Accessing Grafana

## Login via Web UI

1. To access the Grafana UI on the web browser, type the IP along with the port `3000`. For example: ```192.168.122.144:3000```

1. The Grafana sign-in page appears.

1. To sign in to Grafana, enter `admin` for both the username and password.

# Troubleshooting

## Verifying via cURL

Run a curl command to verify whether a given connection should be working in a browser under ideal circumstances.

```bash
curl 192.168.122.144:3000
```

The following example output determines that an endpoint has been located.

```bash
<a href="/login">Found</a>
```

> **Note:**  If you encounter problems in accessing the WebUI then please check the logs for troubleshooting. The default location is `/var/log/grafana/grafana.log`.
> 
> Also make sure that the port `3000` is not blocked by a Firewall.

## Checking log file

1. The default location of grafana log file is `(/var/log/grafana)` directory.
   
   ```bash
   cd /var/log/grafana
   ```

1. Inside there you will find the `grafana.log` file which contains all the logs when having issues e.g. failed to startup or unexpected crash etc. To view it use the command:

   ```bash
   less grafana.log
   ```
   
