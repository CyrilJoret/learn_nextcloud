# Steps

## 1 - On the windows host - Create a SFTP acces

* Install Choco package openssh

<code>choco install openssh
cd "C:\Program Files\OpenSSH-Win64"
powershell.exe -ExecutionPolicy Bypass -File install-sshd.ps1</code>

* Create a new windows user and set privileges on folder to share

* Modify **_C:\ProgramData\ssh\sshd_config_** to limit user to the shared folder

<code>Match User nextcloudread
	ChrootDirectory "z:\"
	X11Forwarding no
	AllowTcpForwarding no
	PermitTTY no
	ForceCommand internal-sftp</code>

## 2 - On the windows host - Set a vagrant VM

* Install a virtualisation solution (VirtualBox, HyperV, Qemu, ....)
  
* Install Vagrant
  
* Create a Vagrant VM with the box **_generic-x64/debian12_** and a bridge network

## 3 - On the Vagrant VM - Start Nextcloud with docker

* SSH in the Vgarant VM
  
* Set a static IP to the VM in **/etc/network/interfaces**

<code>...
iface eth0 inet static
address 192.168.1.102
netmask 255.255.255.0
gateway 192.168.1.1
dns-domain home
dns-nameserver 192.168.1.1
...</code>

* Install docker (apt package docker.io)

<code>sudo apt install docker.io</code>

* Add vagrant user to docker group

<code>sudo usermod -aG docker $USER</code>
  
* Create a container for Postgres :

<code>docker run --restart=always -e POSTGRES_PASSWORD=xxxxxxxxxx -v /var/lib/postgresql/17/docker:/var/lib/postgresql/17/docker -p 5432:5432 -d postgres:17</code>

* Create a container for NextCloud :

<code>docker run --restart=always -v /var/www/html:/var/www/html -p 80:80 -d nextcloud</code>

## 4 - In Nextcloud - Make the first connexion

* Connect to the nextcloud web interface
  
* Set an admin acount and a connexion to the postgres DB
  
* Activate **_External storage support_**

* Add an SFTP external storage with sftp settings created at step 1

## 5 - Acces from internet

* Create a dyndns domain name

* Set NAT ports on router

* Modify trusted_domains in config.php with the dyndns domain name








# TODO
Learn docker compose
Add a Traefik reverse proxy
Active Email
Describe what to backup
Monitoring
Activate HTTPS
