# Week 1 Challenge.

## Launching a websever instance and connecting to the instance.

![Architechture diagram](https://static.us-east-1.prod.workshops.aws/public/b8d66c76-0455-4d13-8acd-9002b999b537/static/images/ec2/amazon-ec2-architecture.svg)

EC2 is a virtual server in Amazon Web Services terminology. With an EC2 instance, AWS subscribers can request and provision a computer server within the AWS cloud.

### Steps for Launching an EC2 Server:-
- Log in to the managment console.
- On the search bar, type EC2 and select it.
- Click launch instance.
- Select the AMI you would Love to use, may it be linux or windows.
- Select the instance type, for use we will be using t2.micro which has 1vCPU and 1GB Memory.
- Generate a key pair, this is used when you want to connect to your instance using SSH.
- Edit network settings; 
	-[] Check default VPC and subnet. Auto-assign public IP is set to Enable.
	-[] Right below it, create Security groups to act as a network firewall.
	-[] Select Add Security group rule and set HTTP to Type.
	-[] Also allow TCP/80 for Web Service by specifying it.
- On the advanced details section Enter the following values in the User data field and select Launch instance.

```
#!/bin/sh
​
#Install a LAMP stack
dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
dnf install -y mariadb105-server
dnf install -y httpd php-mbstring
​
#Start the web server
chkconfig httpd on
systemctl start httpd
​
#Install the web pages for our lab
if [ ! -f /var/www/html/immersion-day-app-php7.zip ]; then
   cd /var/www/html
   wget -O 'immersion-day-app-php7.zip' 'https://static.us-east-1.prod.workshops.aws/public/b8d66c76-0455-4d13-8acd-9002b999b537/assets/immersion-day-app-php7.zip'
   unzip immersion-day-app-php7.zip
fi
​
#Install the AWS SDK for PHP
if [ ! -f /var/www/html/aws.zip ]; then
   cd /var/www/html
   mkdir vendor
   cd vendor
   wget https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip
   unzip aws.zip
fi
​
# Update existing packages
dnf update -y

```
### Browse the Web Server

Wait for the instance to pass the Status Checks to finish loading. Open a new browser tab and browse the Web Server by entering the EC2 instance’s Public DNS name into the browser.
Our website should look like the following. 

![our web server](https://static.us-east-1.prod.workshops.aws/public/b8d66c76-0455-4d13-8acd-9002b999b537/static/images/ec2/ec2-lab-08.png)

