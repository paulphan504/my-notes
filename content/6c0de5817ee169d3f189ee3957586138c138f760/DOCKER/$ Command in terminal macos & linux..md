##### [**Show hidden files and folders within Mac terminal**](https://stackoverflow.com/questions/71983948/show-hidden-files-and-folders-within-mac-terminal)

Have you tried using the -a flag for ls?

1. To list all files (including hidden files): `$ ls -a`
2. To list all files in a list with more details: `$ ls -al`
3. Alias (shortcut) for 2. `$ ll`

`ls` itself usually ignores entries starting with '.', and the `-a` or `--all` flag ignores that protection.

Use `ls --help` in your terminal to review all available flags.

#### [How to CD (move, delete, show) to Your iCloud Drive on a Mac](https://www.wikihow.com/CD-to-iCloud-Drive-on-Mac)

```
cd ~/Library/Mobile\ Documents/
```

![[Pasted image 20241008205715.png]]

##### **Command login ubuntu/linux container with Terminal on macos**.
```
docker ps 
docker exec -u 0 -it "INPUT_NAMES_CONTAINER" bash
```
![[Pasted image 20241013120221.png]]


##### **[Show and change permission directory in linux terminal](https://hcc.unl.edu/docs/handling_data/data_storage/linux_file_permissions/#:~:text=Type%20the%20command%20ls%20%2Dl,a%20file%20or%20a%20directory.)**

permission infomation:
- read = 4
- write = 2
- execute = 1

```
ls -l
chmod 754 "name directory or file"

```
![[Pasted image 20241013224308.png]]

##### [How To View System Users in Linux on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-view-system-users-in-linux-on-ubuntu)

```
less /etc/passwd  "list all user in ubuntu linux"

cut -d : -f 1 /etc/passwd  "list user only name"

less /etc/group  "view all group"

cut -d : -f 1 /etc/group "list group only name"

w "show list user login"

who "show list user login"

sudo whoami "show user running"

groups "show group running"

```

##### [How To Restrict Log In Capabilities of Users on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-restrict-log-in-capabilities-of-users-on-ubuntu)

```
less /etc/passwd | grep messagebus "restrict user with namw"
```

##### [How to Enable SSH with Password Authentication on Ubuntu](https://medium.com/@ravidevops2470/how-to-enable-ssh-with-password-authentication-on-ubuntu-22-04-a7cbdf476d8b)

```
sudo apt update  
sudo apt install openssh-server "Install OpenSSH Server"
sudo nano /etc/ssh/sshd_config "Edit SSH Configuration, reference image show below "
service restart ssh
service start ssh
passwd "change password login ubuntu container"
ssh username@your_server_ip "remote login thought ssh with syntax"
```
![[Pasted image 20241014221457.png]]