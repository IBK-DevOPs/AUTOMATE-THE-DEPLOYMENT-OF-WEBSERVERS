## AUTOMATE-THE-DEPLOYMENT-OF-WEBSERVERS

### Deploying and configuring of webserver and Deploying and configuring load balance

### While implementing load balancer with Nginix, we deployed 2 back end servers, with a load balancer distribution traffic across the webservers.
### We connected the server by typing a code on the terminal with ssh.
### Every process that we did manually will be automated 
### Automation is the heart of the Devops.
### Automation speed up the software deployment and services  and reduce error of doing things manually.

### Importance of Automation in software Development
### Enhanced Efficiency - Automation reduces the manual workload for developers, freeing them up from repetitive tasks such as testing and code review.
### This allows them to focus on more creative and strategic tasks, such as brainstorming new features
### Improved Quality - Automation enhances software quality by reducing human errors and conducting comprehensive testing.
### It runs end-to-end tests for new features, ensuring they work correctly and don’t disrupt existing code.
### Cost Reduction - Automation lowers development costs by minimizing manual work.
### Increased Scalability - Automation makes software development more scalable by automating tasks across various projects and teams.

## DEPLOYING AND CONFIGURING WEBSERVER
### All the process to deploy had been codified in a shell script.
### STEPS TO FOLLOW TO RUN A SCRIP AND CONFIGURING WEBSERVER
### Create EC2 instance running Ubuntu 22.04.
### Create for server 1 and server 2

![alt text](<Image/EC2 created.png>)

### Add a rule to open port 80 and allow traffic from any security group.

![alt text](<Image/Port 80 open.png>)where using 

### Connect servers with a terminal using shh client.
![alt text](<Image/connecting to server 1 via ssh.png>)

![alt text](<Image/connecting 2nd server  EC2 with shh.png>)


### Open a file with below code.
   ### ` sudo vi install.sh`

![alt text](<Image/sudo vi install.png>)

 ### Open a file and paste the script below .
### After inputting public ip click escape.

![alt text](<Image/server 1 scrip.png>)

![alt text](<Image/server 2 scrip.png>)

### Press wq! To safe the script.

### Replace server 1  PUBLIC_IP with public IP (Public IP:13.58.17.91)

### Replace server 2 PUBLIC_IP with public IP (Public IP:18.191.175.48)


### `#!/bin/bash

### ####################################################################################################################
#####
 ### This automates the installation and configuring of apache webserver to listen on port 8000
##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below:
########
 `./install_configure_apache.sh 127.0.0.1`
####################################################################################################################

`set -x # debug mode`
`set -e # exit the script if there is an error`
`set -o pipefail # exit the script when there is a pipe failure`

PUBLIC_IP=$1

`[ -z "${PUBLIC_IP}" ] && echo "Please pass the`
`public IP of your EC2 instance as an argument to `the script" && exit 1`

`sudo apt update -y &&  sudo apt install apache2 -y`

`sudo systemctl status apache2`

`if [[ $? -eq 0 ]]; then`
   ` sudo chmod 777 /etc/apache2/ports.conf`
   ` echo "Listen 8000" >> /etc/apache2/ports.conf`
    `sudo chmod 777 -R /etc/apache2/`

    `sudo sed -i 's/<VirtualHost \*:80>/`<VirtualHost *:8000>/' /etc/apache2/`sites-available/000-default.conf

`fi
`sudo chmod 777 -R /var/www/
`echo "<!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: "${PUBLIC_IP}"</p>
        </body>
        </html>" > /var/www/html/index.html

`sudo systemctl restart apache2`


Change the permission of the script.
Run below code

   `![alt text](<Image/To change the permission chmod.png>) sudo chmod +x install.sh`


Run shell script by running below code
  ` ./install.sh PUBLIC_IP`
### Repeat this step with server 2, run the script and save.

### DEPLOYMENT OF NGINIX AS LOAD BALANCER
### Create EC2 instance running Ubuntu 22.04.

![alt text](<Image/EC2 created.png>)

### Create for server for Nginix 

### Add a rule to open port 80 and allow traffic from anywhere using security group.

![alt text](<Image/Port 80 open.png>)

### Connect servers with a terminal using shh client.

![alt text](<Image/connect nginix with shh.png>)


### STEPS TO FOLLOW TO RUN A SCRIP AND CONFIGURING LOAD BALANCE NGINIX
### Create EC2 instance running Ubuntu 22.04.
### Create for server NGINIX server
### Add a rule to open port 80 and allow traffic from anywhere using security group.
### Connect servers with a terminal using shh client.
### Open a file with below code.

   ` sudo vi install.sh`

![alt text](<Image/Load balancer scrip with public IP.png>)

### Open a file and paste the script below .
### Replace server 1  PUBLIC_IP with public IP (Public IP:13.58.17.91)
### Replace server 2 PUBLIC_IP with public IP (Public IP:18.191.175.48)
### Replace server LOAD BALANCE PUBLIC_IP with public IP                                           
   ### (Public IP: 18.217.222.236)

![alt text](<Image/Nginix load balancer script A.png>)

![alt text](<Image/Nginix load balancer script B.png>)

### After inputting public ip click escape
### Press type esc the shift + :wqa! To safe the script.
### Change permission to make the script executable with below code
`sudo chmod +x nginx.sh`


![alt text](<Image/Nginix chmod.png>)


###  Run the script with below code
`./nginx.sh PUBLIC_IP Webserver-1 Webserver-2`

### VERIFYING SERVER 1 SETUP

![alt text](<Image/welcome to  EC2 server 1.png>)


### VERIFYING SERVER 2 SETUP

![alt text](<Image/welcome to EC2 server 2.png>)

### VERIFYING LOAD BALANCER SETUP

![alt text](<Image/Welcome to load balancer.png>)

### INPUT PUBLIC IP OF A SERVER 1 &  2 AND LOAD
### BALANCER  ON A URL
### WE WILL SEE WELCOME TO EC2 AND INGINIX LOAD BALANCER.









