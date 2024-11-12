- **[Open link safe](https://youtube.com/shorts/ElQxbbNBayI?si=SYOIFyCQ8Y9axI7o)**

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
[prometheus-2.55.0-rc.0.linux-amd64.tar.gz](https://github.com/prometheus/prometheus/releases/download/v2.55.0-rc.0/prometheus-2.55.0-rc.0.linux-amd64.tar.gz)
tar xvfz node_exporter-*.*-amd64.tar.gz
cd node_exporter-*.*-amd64
./node_exporter
```

- **[Cartoon energy (The Wild Robot):](https://motchilltc.us/xem/robot-hoang-da-the-wild-robot/full)** [link](https://motchilltc.us/xem/robot-hoang-da-the-wild-robot/full)

- [Orbstack intruduce method for replace Docker Desktop](https://www.youtube.com/watch?v=aJe7CvQ-aM8)

- [VScode intruduce](https://www.youtube.com/watch?v=1ZfO149BJvg&t=208s)

- [Intruduce youself trainer by youtuber Lucy](https://www.youtube.com/watch?v=QgjkjsqAzvo)

- [improve lisening english skill native](https://youtube.com/watch?v=D6_qpaSxAQc&si=x7c1gERtsJ7j5B96 )
  
- Pi-Hole vs Adguard

- PFSence https://youtube.com/watch?v=lUzSsX4T4WQ&si=uFU0_d0-VQN4MRJK


- Zabbix
- [ ] go to home (@2024-11-01 11:59)
- [ ] Relax (@2024-11-01 11:58)

  docker run -d \
  --name=kali \
  --security-opt seccomp=unconfined \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Etc/UTC \
  -p 3000:3000 \
   -p 3001:3001 \
  --shm-size="1gb" \
  --restart unless-stopped \
lscr.io/linuxserver/kali-linux:latest
- [ ] Practice mounth and void sound(@2024-11-08 09:00) [link](https://www.youtube.com/watch?v=l69yZ5xabbo&t=3s)

# Minimal command
docker run -d --name pihole --network=host -e WEBPASSWORD="YourPassword" -e DNS1=1.1.1.1 -p 80:80 -p 53:53/tcp -p 53:53/udp -p 443:443 pihole/pihole:latest

# Recommended command
docker run -d --name pihole -e ServerIP=172.16.18.86 -e TZ=Europe/Helsinki -e WEBPASSWORD=SecretAgent007 -e DNS1=1.1.1.1 -e DNS2=1.0.0.1 -p 80:80 -p 53:53/tcp -p 53:53/udp -p 443:443 -v ~/pihole/:/etc/pihole/ --dns=127.0.0.1 --dns=1.1.1.1 --cap-add=NET_ADMIN --restart=unless-stopped pihole/pihole:latest




