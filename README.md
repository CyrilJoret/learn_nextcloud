# Steps

## On the host - Set a vagrant VM

* Install a virtualisation solution (VirtualBox, HyperV, Qemu, ....)
  
* Install Vagrant
  
* Create a Vagrant VM with the box **_generic-x64/debian12_** and a bridge network

## On the Vagrant VM - Start Nextcloud with docker

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

<code>docker run -e POSTGRES_PASSWORD=xxxxxxxxxx -v /var/lib/postgresql/18/docker:/var/lib/postgresql/18/docker -p 5432:5432 -d postgres:18</code>

* Create a container for NextCloud :

<code>docker run -v /var/www/html:/var/www/html -p 80:80 -d nextcloud</code>

## In Nextcloud - Make the first connexion

* Connect to the nextcloud web interface
  
* Set an admin acount and a connexion to the postgres DB
  
* Activate **_External storage support_**


