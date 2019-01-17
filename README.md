# Linux-server-configuration-
* This project is part of Udacity full stack nanodegree.We configr linux server, we will use google cloude to make VM.

* IP address:35.196.5.159 
* Accessible SSH port: 2200
* Application uri : http://35.196.5.159.xip.io

## First steps:
* Make VM instances on google cloud
* on our computer we will generate key pairs $ ssh-keygen 
* Copy public key on ssh keys part on VM instance
* contact to the server `$ ssh -i ~/.ssh/my-ssh-key root@35.196.5.159`

## Secure the server:
- We need to update & upgrade packegs 
* sudo apt-get update 
* sudo apt-get upgrade

## Change ssh port to 2200
* `$ sudo nano /etc/ssh/sshd_config`
* Looking for 22 and change it to 2200
* Restart SSH: `$ sudo service ssh restart`

## Configure firewall (ufw):
* `sudo ufw status`
* `sudo ufw default deny incoming`
* `sudo ufw default allow outgoing`
* `sudo ufw allow 2200/tcp`
* `sudo ufw allow www`
* `sudo ufw allow 123/udp`
* `sudo ufw deny 22`

now enable UFW `sudo ufw enable`
