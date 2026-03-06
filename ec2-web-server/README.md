
# AWS EC2 Web Server Lab

## Objective
Launch an EC2 instance and host a simple website using Apache.

## AWS Services Used
- EC2
- Security Groups

## Steps

1. Login to AWS Console
2. Go to EC2 Dashboard
3. Click Launch Instance
4. Choose Amazon Linux AMI
5. Configure Security Group (Allow HTTP - Port 80)

## Install Apache

sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd

## Add Sample Website

echo "<h1>Hello from AWS EC2</h1>" > /var/www/html/index.html

## Output
Website hosted on EC2 Public IP
