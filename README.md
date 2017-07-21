# LinuxServer-Configration
LinuxServer-Configration udacity project 
type 
ssh user 
password 654321


---------------------------------------
summary of software installed
---------------------------------------
installing apache server on machine 
$ sudo apt-get install apache2

install mod_wsgi
$ sudo apt-get install libapache2-mod-wsgi
Enable mod_wsgi 
$ sudo a2enmod wsgi

install PostgreSQL
run sudo apt-get install postgresql

install git
$ sudo apt-get install git

---------------------------------------
summary of configurations
---------------------------------------
Update all currently installed packages
$ sudo apt-get update
$ sudo apt-get upgrade

Change the SSH port from 22 to 2200
sudo nano modify /etc/ssh/sshd_config
and change port to 2200
restart ssh sudo service ssh restart

------------------------------------------
Configure the Uncomplicated Firewall (UFW) 
$ sudo ufw status 
$ sudo ufw enable 

allow to port 80
$ sudo ufw allow 80/tcp

allow to port 123 NTP
$ sudo ufw allow 123/udp

allow to port 2200
$ sudo ufw allow 2200/tcp

add user grader 
$ sudo adduser grader 

set password to user
$ sudo passwd grader

Give grader the permission to sudo
$ sudo usermod -a -G sudo hduser

-------------------------------------
Configure the local timezone to UTC
sudo dpkg-reconfigure tzdata

Disable ssh login for root user
sudo nano /etc/ssh/sshd_config

add this 
PermitRootLogin no
then sudo service ssh restart


Configure key-based authentication for grader user
ssh-keygen
location is   /home/grader/.ssh/authorized_keys
passwd 123456

start the apache server 
sudo service apache2 start




