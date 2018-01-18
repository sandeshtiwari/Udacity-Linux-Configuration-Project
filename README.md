1) Launch Virtual Machine
	-Download  the private key
	-move the file into the ~/.ssh folder.
	-Open the terminal and type chmod 600 ~/.ssh/LightsailDefaultPrivateKey-us-east-2.pem
	-In your terminal type ssh -i ~/.ssh/LightsailDefaultPrivateKey-us-east-2.pem ubuntu@18.218.155.245
	-Development environment
		- Public IP - 18.218.155.245
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
		- ssh -i ~/.ssh/linuxConfig grader@18.218.155.245
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

