# LinuxServer-Configration

## IP address http://35.190.157.186/  and SSH port = 2200 
  
## SSH key you created for the grader user.
    $ ssh  grader@35.190.157.186 -p 2200 -i [RSA_FILE]
      
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

    $ sudo apt-get install postgresql

install git

     $ sudo apt-get install git

---------------------------------------
summary of configurations
---------------------------------------
### Update all currently installed packages
 
    $ sudo apt-get update
    $ sudo apt-get upgrade

Change the SSH port from 22 to 2200

    $ sudo nano modify /etc/ssh/sshd_config
and change port to 2200

    $restart ssh sudo service ssh restart

------------------------------------------
## Configure the Uncomplicated Firewall (UFW) 

    $ sudo ufw status 
    $ sudo ufw enable 


## allow to port 80 , 123 NTP , 2200

    $ sudo ufw allow 80/tcp
    
    $ sudo ufw allow 123/udp
    
    $ sudo ufw allow 2200/tcp
    
    
    
  ## add user grader   
    
    $ sudo adduser grader 
    
## set password to user
   
    $ sudo passwd grader

## Give grader the permission to sudo
    $ sudo usermod -a -G sudo hduser


-------------------------------------
## Configure the local timezone to UTC
   
    $ sudo dpkg-reconfigure tzdata

Disable ssh login for root user

    $ sudo nano /etc/ssh/sshd_config

add this 

    PermitRootLogin no
then 

    $ sudo service ssh restart
    
Configure key-based authentication for grader user
 
    $ ssh-keygen
    
####  on server config the authorized keys
    $ mkdir .ssh
    $ touch .ssh/authorized_keys
    $ chmod 700 .ssh
    $ chmod 644 .ssh/authorized_keys
  
## Make a catalog named directory in /var/www

    $ sudo mkdir /var/www/catalog

## Clone the itemCatalog to the catalog directory:
    $ git clone https://github.com/nadaadel/ItemCatalog.git

## Make a catalog.wsgi file :
    $ sudo touch catalog.wsgi 
    $ sudo nano catalog.wsgi

## Edit catalog.wsgi file :   
    import sys
    import logging
    logging.basicConfig(stream=sys.stderr)
    sys.path.insert(0, "/var/www/catalog/")
    from project import app as application
    
## Edit the default Virtual File :
   $  sudo nano /etc/apache2/sites-available/000-default.conf
   
        <VirtualHost *:80>
          ServerName XX.XX.XX.XX
          ServerAdmin nona6066@gmail.com
          WSGIScriptAlias / /var/www/catalog/catalog.wsgi
          <Directory /var/www/catalog/>
              Order allow,deny
              Allow from all
          </Directory>
          Alias /static /var/www/catalog/static
          <Directory /var/www/catalog/static/>
              Order allow,deny
              Allow from all
          </Directory>
          ErrorLog ${APACHE_LOG_DIR}/error.log
          LogLevel warn
          CustomLog ${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>

## config database postgresql

    psql
    CREATE USER catalog WITH PASSWORD 'password';
    ALTER USER catalog CREATEDB;
    CREATE DATABASE catalog WITH OWNER catalog;
    \c catalog
    REVOKE ALL ON SCHEMA public FROM public;
    GRANT ALL ON SCHEMA public TO catalog;
    \q
    exit
    

#### edit project.py , lotsofmenus.py , database setup.py database connection :
    engine = create_engine('postgresql://catalog:password@localhost/catalog')
 
## run database setup 
    python /var/www/catalog/catalog/database_setup.py
    
## insert dummy data to database 
    python lotsof menus.py 
    

## start the apache server 
    $ sudo service apache2 start




