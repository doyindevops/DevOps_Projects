# Automating Load Balancer Configuration with Shell Scripting

In PROJECT 7 we manually set up load balancers which was an easy task because we provisioned 2/3  servers. However, working on multiple servers this would be very complex and repetitive which will make manual process of provisioning to be susceptible to errors and highly inefficient.

As a DevOps Engineer automation is at the heart of our processes 
So in this project we will be streamlining our load balancer configuration with ease using shell scripting and simple CICD on Jenkins. Wwe will automate the set up and maintenance of our load balancer with shell scripting and freestyle job. This will enhance efficiency and reduce manual effort.



### Automate the Deployment of Webservers
In order to increase the speed of deployment, the entire process in the PROJECT 7 will be automated. We will write a shell scripts that will automate the entire processes, which will replace typing commands in our terminal to deploy 2 backend Apache servers with an Ngnix load balancer distributing traffic across the webservers.


## Deploying and Configuring the Webservers

All the process we need to deploy our webservers has been codified in the shell script below:

>
>   #!/bin/bash
.
>##################################################################>##################################################
>##### This automates the installation and configuring of apache >webserver to listen on port 8000
>##### Usage: Call the script and pass in the Public_IP of your >EC2 instance as the first argument as shown below:
>######## ./install_configure_apache.sh 127.0.0.1
>##################################################################>##################################################
>
>set -x # debug mode
>set -e # exit the script if there is an error
>set -o pipefail # exit the script when there is a pipe failure
>
>PUBLIC_IP=$1
>
>[ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your >EC2 instance as an argument to the script" && exit 1
>
>sudo apt update -y &&  sudo apt install apache2 -y
>
>sudo systemctl status apache2
>
>if [[ $? -eq 0 ]]; then
>
>     sudo chmod 777 /etc/apache2/ports.conf
>     echo "Listen 8000" >> /etc/apache2/ports.conf
>     sudo chmod 777 -R /etc/apache2/
>
>     sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/  > apache2/ >sites-available/000-default.conf
>
>fi
>   
>   sudo chmod 777 -R /var/www/
>
>   echo "<!DOCTYPE html>
>
>        <html>
>        <head>
>            <title>My EC2 Instance</title>
>        </head>
>        <body>
>
>           <h1>Welcome to my EC2 instance</h1>
>            <p>Public IP: "${PUBLIC_IP}"</p>
>        </body>
>        </html>" > /var/www/html/index.html
>
>sudo systemctl restart apache2
>


#### Follow the steps below to run the Apache2 webserver shell scripts:

##### Step 1. 
Provision an EC2 instance running ubuntu 20.04. You can refer to ***Project 7*** as a refresher. 
**Note: We are provisioning 2 webservers**

![Alt text](<Images/WEBSERVER 1.png>)

![Alt text](<Images/2 INSTANCE RUNNING.png>)

#### Step 2.
Open port 8000 to allow traffic from anywhere using the security group. 

![Alt text](<Images/INBOUND 8000.png>)

#### Step 3.
Connect to the webservers via the terminal using SSH client

![Alt text](<Images/SSH INTO WEBSERVER.png>)

#### Step 4. 
Open a file, and paste the shell script above and close the file after editing
***NOTE: Replace the placeholders with the webservers public IP addresses***

![Alt text](<Images/VI INSTALL.SH.png>)

To close the file press **esc** and the press **shift + :wq!**


#### Step 5. 
Change the permissions on the file to make it executable using the command below:

>       sudo chmod +x install.sh

![Alt text](<Images/chmod 1.png>)

#### Step 6.
Run the shell script using the command below.

>       ./install.sh PUBLIC_IP

![Alt text](<Images/AUTO RUN 1.png>)
![Alt text](<Images/AUTO RUN 1B.png>)

***NOTE: We are to go through the same steps for the 2 apache2 webservers**




## Automate the Deployment of Nginx as a Load Balancer using Shell Script

We have successfully deployed and configured the two webservers, we can now proceed to also install and configure the load balancer.

As with the webservers, we need to launch an EC2 instance on AWS running on ubuntu 22.04. Then open port 80 anywhere using the security group and connect to load balancer via the terminal.

***NOTE: Don't use the same security group for the Loadbalancer, you will have errors in gateways***

### Deploying and Configuring Nginx Load Balancer
The process to deploy and configure our load balancer is codified in the below shell script.


>
    #!/bin/bash

    ######################################################################################################################
    ##### This automates the configuration of Nginx to act as a load balancer
    ##### Usage: The script is called with 3 command line arguments. The public IP of the EC2 instance where Nginx is installed
    ##### the webserver urls for which the load balancer distributes traffic. An example of how to call the script is shown below:
    ##### ./configure_nginx_loadbalancer.sh PUBLIC_IP Webserver-1 Webserver-2
    #####  ./configure_nginx_loadbalancer.sh 127.0.0.1 192.2.4.6:8000  192.32.5.8:8000
    ############################################################################################################# 

    PUBLIC_IP=$1
    firstWebserver=$2
    secondWebserver=$3

    [ -z "${PUBLIC_IP}" ] && echo "Please pass the Public IP of your EC2 instance as the argument to the script" && exit 1

    [ -z "${firstWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the second argument to the script" && exit 1

    [ -z "${secondWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the third argument to the script" && exit 1

    set -x # debug mode
    set -e # exit the script if there is an error
    set -o pipefail # exit the script when there is a pipe failure


    sudo apt update -y && sudo apt install nginx -y
    sudo systemctl status nginx

    if [[ $? -eq 0 ]]; then
    sudo touch /etc/nginx/conf.d/loadbalancer.conf

    sudo chmod 777 /etc/nginx/conf.d/loadbalancer.conf
    sudo chmod 777 -R /etc/nginx/

    
    echo " upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server  "${firstWebserver}"; # public IP and port for webserser 1
            server "${secondWebserver}"; # public IP and port for webserver 2

            }

           server {
            listen 80;
            server_name "${PUBLIC_IP}";

            location / {
                proxy_pass http://backend_servers;   
            }
    } " > /etc/nginx/conf.d/loadbalancer.conf
    fi

    sudo nginx -t

    sudo systemctl restart nginx






## Steps to Run the Shell Script

#### Step 1.
Provision an EC2 Instance running on ubuntu 22.04

![Alt text](<Images/NGINX WEBSERVER.png>)

Open port 80 to anywhere using a security group 

![Alt text](<Images/INBOUND 80.png>)

Connect to the load balancer via terminal

![Alt text](<Images/NGINX INSTANCE RUNNING.png>)


#### Step 2.
On your terminal, open a file nginx.sh using the command below:

>           sudo vi nginx.sh

Copy and Paste the script above inside the file ( Replace all the placeholders with appropriate IP's. Ensure you study the instructions)

![Alt text](<Images/LOADBALANCER VI.png>)

#### Step 3.

Close the file with
**type esc then shift + :wq!**

#### Step 4. 
Change the file permission to make it an excutable one using the below command:

>       sudo chmod +x nginx.sh

#### Step 5. 
Run the Script with the command below ( Replace PUBLIC_IP with the Loadbalancer IP and the IP addresses for both Apache2 webservers'IP)

>       ./nginx.sh PUBLIC_IP Webserver-1 Webserver-2

![Alt text](<Images/LOADBA RUN1.png>)

![Alt text](<Images/LOADBA RUN2.png>)

Now we verify if our Automation was successful by opening each IP on a browser. And you should have outputs similar to the below:

Apache Webserver 1
![Alt text](<Images/BROWSER 1.png>)

Apache Webserver 2
![Alt text](<Images/BROWSER 2.png>)

Nginx Load balancer
![Alt text](<Images/BROWSER 3.png>)


