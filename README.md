Project Description:
Take a baseline installation of a Linux distribution on a virtual machine and prepare it to host your web
applications, to include installing updates, securing it from a number of attack vectors and 
installing/configuring web and database servers
1) Launch Virtual Machine
	-Download  the private key
	-move the file into the ~/.ssh folder.
	-Open the terminal and type chmod 600 ~/.ssh/LightsailDefaultPrivateKey-us-east-2.pem
	-In your terminal type ssh -i ~/.ssh/LightsailDefaultPrivateKey-us-east-2.pem ubuntu@18.218.171.145
	-Development environment
		- Public IP - 18.218.171.145
		- Private Key(not provided)
2) Create a new user named grader
	- sudo adduser grader
	- sudo nano /etc/sudoers.d/grader
		- type this in grader ALL=(ALL:ALL) ALL, save and quit
3) Generate keys on the local machine using ssh-keygen, and save the private key in the ~/.ssh on the local computer
4) On the remote server log in as grader using su - grader and do the following:
	- mkdir .ssh
	- touch .ssh/authorized_keys
5) On the local machine open the .pub file from the .ssh directory and copy paste everything to the
.ssh/authorized_keys file in the server, save and quit.
6) Run the following commands on your remote server:
	- chmod 700 .ssh
	- chmod 644 .ssh/authorized_keys
	- Now login to the server using this command
		- ssh -i ~/.ssh/myKey.rsa grader@18.218.171.145
7) Update all packages currently installed
	- sudo apt-get update
	- sudo apt-get upgrade
8) Configure Uncomplicated Firewall(UFW) to only allow connection for SSH(port 22), HTTP(port 80), and NTP(port 123)
	- sudo ufw allow 22/tcp
	- sudo ufw allow 80/tcp
	- sudo ufw allow 123/udp
	- sudo ufw enable
9) Configure the local timezone
	- sudo dpkg-reconfigure tzdata
10) Install and configure Apache to serve a Python mod_wsgi application
	- Install Apache with sudo apt-get install apache2
	- Install mod_wsgi sudo apt-get python-setuptools libapache2-mod-wsgi
	- Restart Apache sudo service apache2 restart
11) Install and config PostgreSQL
	- sudo apt-get install postgresql
	- Login as user 'postgres' using sudo su - postgres
	- Get to the postgreSQL shell using psql command
	- Create a database a nd a new user
		- CREATE DATABASE restaurantmenu;
		- CREATE USER admin;
	- Set password for the admin user
		- ALTER ROLE admin WITH PASSWORD 'admin';
	- Give user 'admin' permission to 'restaurantmenu' applicaition database
		- GRANT ALL PRIVILEGES ON DATABASE restaurantmenu to admin
	- Exit postgreSQL shell using \q
	- Exit from 'postgres' user using 'exit' command
12) Install git and setup the RestaurantMenu app project 
	- Install Git using sudo apt-get install git
	- cd /var/www
	- create an app directory using sudo mkdir FlaskApp
	- cd FlaskApp
	- Clone the app from GitHub repo
		- sudo git clone https://github.com/sandeshtiwari/Restaurant-Menu-App.git
	- Rename the file using sudo mv ./Restaurant-Menu-App ./FlaskApp
	- cd to inner FlaskApp using cd FlaskApp
	- Edit database_setup.py and finalProject.py and change engine = create_engine('sqlite:///restaurantmenuwithusers.db')
	  to engine = create_engine('postgresql://admin:admin@localhost/restaurantmenu')
	- Rename finalProject.py to __init__.py by using sudo mv finalProject.py __init__.py
	- Install pip with sudo apt-get install python-pip
	- Install Flask sudo -H pip install Flask
	- Install other project dependencies
		- sudo -H pip install httplib2 oauth2client sqlalchemy psycopg2 sqlalchemy_utils requests
	- Create database shema using sudo python database_setup.py
13) Create the .wsgi file
	- cd /var/www/FlaskApp
	- sudo nano flaskapp.wsgi
	- Add the following code to the flaskapp.wsgi file
		#!/usr/bin/python
		import sys
		import logging
		logging.basicConfig(stream=sys.stderr)
		sys.path.insert(0,"/var/www/FlaskApp/")

		from FlaskApp import app as application
		application.secret_key = 'super_secret_key'
14) Configure 'nano /etc/apache2/sites-available/FlaskApp.conf' and add the following
	<VirtualHost *:80>
		ServerName 18.218.155.245
		ServerAdmin sandeshtiwari@live.com
		WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
		<Directory /var/www/FlaskApp/FlaskApp/>
			Order allow,deny
			Allow from all
		</Directory>
		Alias /static /var/www/FlaskApp/FlaskApp/static
		<Directory /var/www/FlaskApp/FlaskApp/static/>
			Order allow,deny
			Allow from all
		</Directory>
		ErrorLog ${APACHE_LOG_DIR}/error.log
		LogLevel warn
		CustomLog ${APACHE_LOG_DIR}/access.log combined
	</VirtualHost>
	- Enable the host using sudo a2ensite FlaskApp
	- sudo service apache2 restart
15) Change the path for the client_secrets.json file everywhere
	'/var/www/FlaskApp/FlaskApp/client_secrets.json'
16) Make sure that the client secrets file matches the Authorized JavaScript origins on the credentials page.
	- Add the IP address to the list, eg. by visiting https://console.cloud.google.com/apis/credentials
	- Also update the client_secret.json file or redownload it after adding the IP by visiting the same URL
17) Disable root login:
	- run sudo nano/etc/ssh/sshd_config
	- change the PermitRootLogin without-password to PermitRootLogin no
	- Also make sure to uncomment the "PasswordAuthentication no" line
18) Change the SSH port from 22 to 2200
	- use sudo nano /etc/ssh/sshd_config and then change the Port 22 to Port 2200
	- use sudo service ssh restart
	- Make sure that port 2200 has been added to the external Lightsail firewall under the "Networking" tab.
	- Make sure you remove SSH port 22 from the 'Networking' tab
External references: https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
