**Reference:**

[Link](https://dev.to/brownian77/setting-up-a-local-development-environment-with-docker-on-mac-and-windows-peb)

- ### Setting Up on Mac

Step 1: Install Docker Desktop

1. Download Docker Desktop for Mac from the [official Docker website](https://docs.docker.com/get-docker).
2. Double-click the downloaded `.dmg` file to open the installer.
3. Drag the Docker icon to the Applications folder to install Docker Desktop.
4. Open Docker Desktop from the Applications folder.

Step 2: Pull Ubuntu Image

Open Terminal and run the following command to pull the Ubuntu image from Docker Hub:  

```
docker pull ubuntu
```

Step 3: Run Ubuntu Container

Run the following command to start a Docker container based on the Ubuntu image:  

```
docker run -it --name my-ubuntu-container ubuntu
```

Step 4: Restarting the Container

To restart the container named "my-ubuntu-container" later, use the following command:  

```
docker start my-ubuntu-container
```

Step 5: Login linux container [[$ Command in terminal macos & linux.]]

Step 6: install libraries nessecseries for linux container
```
apt update
apt upgrade
sudo apt install net-tools
apt install iproute2 -y "install control interface network"
apt install wget
apt install sudo
apt install systemctl
apt install chown
apt install nano
sudo apt install inetutils-ping "install ping services"
```