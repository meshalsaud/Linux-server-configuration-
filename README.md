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

## Use [Fail2Ban](http://www.fail2ban.org/wiki/index.php/Main_Page) to ban attackers:
* `sudo apt-get install fail2ban`
* Install sendmail for email notice: `sudo apt-get install sendmail iptables-persistent`
* Create a copy of a jail file: `sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local`
* Change `destemail` on jail.local to your email `destemail = useremail@domain`
* Under [sshd] change port = ssh by port = 2200.
* `sudo service fail2ban restart`

## Automatically install updates:
* `sudo apt-get install unattended-upgrades`
* `sudo dpkg-reconfigure -plow unattended-upgrades`

## Create Grader user:
* `$ sudo adduser grader`
  ### Give `grader` the permission to sudo:
  * `$ sudo visudo`
  * Search for the line that looks like this:
   `root    ALL=(ALL:ALL) ALL`
  * add grader user:
   ```
   root    ALL=(ALL:ALL) ALL
   grader  ALL=(ALL:ALL) ALL
   ```

##  Configure the local timezone to UTC
* run `sudo dpkg-reconfigure tzdata`
THEN 
* Choose 
```
None of them
UTC
```

## Install and configure Apache:
* While logged in as grader `$ sudo apt-get install apache2` 
* Install mod_wsgi `$ sudo apt-get install python-setuptools libapache2-mod-wsgi`
* `$ sudo service apache2 restart`

## Install and configure PostgreSQL
* While logged in as grader `$ sudo apt-get install postgresql`
* Switch to the postgres `$ sudo su - postgres`
* `psql`
* CREATE ROLE catalog WITH LOGIN PASSWORD 'catalog';
* ALTER ROLE catalog CREATEDB;
* See list current roles and their attributes: # \du;
* REVOKE ALL ON SCHEMA public FROM public;
* GRANT ALL ON SCHEMA public TO catalog;
* `\q` then `exit`
* Run `$ python database_setup.py`
