**Run in Terminal macos**

- Istall *libreoffice*
	```
	docker run -d \
	--name=libreoffice \
	--security-opt seccomp=unconfined \
	-e PUID=1000 \
	-e PGID=1000 \
	-e TZ=Etc/UTC \
	-p 3005:3000 \
	-p 3006:3001 \
	--restart unless-stopped \
	lscr.io/linuxserver/libreoffice:latest
	```
- Istall *Obsidian*
  ```
	docker run -d \
	--name=obsidian
	-security-opt seccomp=unconfined '#optional' \
	-e PUID=1000 \
	-e PGID=1000 \
	-e TZ=Etc/UTC \
	-p 3003:3000 \
	-p 3004:3001 \
	--shm-size="1gb" \
	--restart unless-stopped \
	lscr.io/linuxserver/obsidian:latest
	```