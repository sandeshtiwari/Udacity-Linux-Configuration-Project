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

		