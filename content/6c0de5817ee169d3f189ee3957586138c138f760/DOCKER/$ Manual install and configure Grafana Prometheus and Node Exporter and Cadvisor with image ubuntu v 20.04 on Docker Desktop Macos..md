##### [ Install Prometheus Server on Ubuntu 20.04](https://docs.vultr.com/install-prometheus-server-on-ubuntu-20-04):
 ##### **Install linux ubuntu v20.04.**
 
 ```
 docker run -it --name prometheus -p 9090:9090 ubuntu:20.04 "remember pull image ubuntu:20.04 in docker desktop befor run this command for create new container run ubuntu os "
```
- Login container with terminal on macos.
```
docker exec -it prometheus /bin/sh 
```

##### 1. Update the System

Update the apt package list to prepare the system for further installations.

```
$ sudo apt update
```

##### 2. Download and Install Prometheus

Prometheus installation files are packaged as precompiled binaries. To download your preferred binaries, you can visit the official Prometheus [download page](https://prometheus.io/download/).

If you decide to install a different version of Prometheus, please note the version numbers in the following examples when downloading and extracting the archives.

Download the Prometheus release package.

```
$ wget https://github.com/prometheus/prometheus/releases/download/v2.27.1/prometheus-2.27.1.linux-amd64.tar.gz
```

Extract the downloaded archive.

```
$ tar xvf prometheus-2.27.1.linux-amd64.tar.gz
```

Change directory to the extracted archive.

```
$ cd prometheus-2.27.1.linux-amd64
```

Create the configuration file directory.

```
$ sudo mkdir -p /etc/prometheus
```

Create the data directory.

```
$ sudo mkdir -p /var/lib/prometheus
```

Move the binary files `prometheus` and `promtool` to `/usr/local/bin/`.

```
$ sudo mv prometheus promtool /usr/local/bin/
```

Move console files in `console` directory and library files in `console_libraries` directory to `/etc/prometheus/` directory.

```
$ sudo mv consoles/ console_libraries/ /etc/prometheus/
```

Move the template configuration file `prometheus.yml` to `/etc/prometheus/` directory

```
$ sudo mv prometheus.yml /etc/prometheus/prometheus.yml
```

Verify the installed version of Prometheus.

```
$ prometheus --version
```

Verify the installed version of promtool.

```
$ promtool --version
```

##### 3. Configure System Group and User

Create a `prometheus` group.

```
$ sudo groupadd --system prometheus
```

Create a user `prometheus` and assign it to the created `prometheus` group.

```
$ sudo useradd -s /sbin/nologin --system -g prometheus prometheus
```

Set the ownership of Prometheus files and data directories to the `prometheus` group and user.

```
$ sudo chown -R prometheus:prometheus /etc/prometheus/  /var/lib/prometheus/

$ sudo chmod -R 775 /etc/prometheus/ /var/lib/prometheus/
```

##### 4. Configure Systemd Service

Create a systemd service file for Prometheus to start at boot time.

```
$ sudo nano /etc/systemd/system/prometheus.service
```

Add the following lines to the file and save it:

```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Restart=always
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.listen-address=0.0.0.0:9090

[Install]
WantedBy=multi-user.target
```

Start the Prometheus service.

```
$ sudo systemctl start prometheus
```

Enable the Prometheus service to run at system startup.

```
$ sudo systemctl enable prometheus
```

Check the status of the Prometheus service.

```
$ sudo systemctl status prometheus
```

##### Access Your Server

Access the Prometheus interface through your browser at port 9090. For example:

```
http://localhost:9090
```
