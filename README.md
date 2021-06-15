# Cloud Computing

## What is cloud computing

Cloud computing is the on-demand availability of computer system resources, especially data storage and computing power, without direct active management by the user. The term is generally used to describe data centers available to many users over the Internet.

### Some benefits

- Reduce IT Costs
- Scalability
- Collaboration efficiency
- Flexibility of work practices
- Access to automatic updates
- More Reliable and Secure

### Best use cases

- When the company is going to grow exponentially
- When working with big data
- When cloud storage is necessary
- For data backup

### Who is using cloud computing in the industry

- Amazon
- Netflix
- Ebay
- Twitch

## What is AWS

AWS is a comprehensive. evolving cloud computing platform provided by Amazon.

- SaaS (Software as a Service)
- laaS (Infrastructure as a Service)
- PaaS (Platform as a Service)

### Some benefits

- Go global in minutes
- Stop spending money running and maintaining data centers
- Increase speed and agility
- Stop guessing capacity
- Pay only for how much you consume

### Who is using AWS in the industry

- Facebook
- Twitter
- Linkedin
- Mcdonalds

## How to create an EC2 instance

1. Choose EC2 in AWS.
2. Select server (Ubuntu Server 16.04...)
3. Select t2.micro
4. Configure instance details
5. Leave network as default
6. Select the DevOpsStudents subnet
7. Auto-assign Public IP: Enable
8. Add storage
9. Choose the volume type and how much storage you need
10. Add tags (important)
11. Configure security group
12. For SSH under Source column select My IP
12. Add a new rule of type HHTP and under the Source column set it to Anywhere.
13. Review and launch.

## Deploy node app

1. Open AWS and a local terminal
2. Select the instance created
3. Under the SSH client column you will find information on how to connect to SSH
4. Give permisions with `400 devop_bootcamp.pem`
5. Next, ssh into your VM using the ssh command provided in AWS: ssh -i "devop_bootcamp.pem" user@awsVirtualMachineLink.com
6. After succesfully ssh into VM do:
			- sudo apt-get update -y
			- sudo apt-get upgrade -y
			- sudo apt-get install nginx
7. In another terminal enter in the folder where app file is and do scp -i devop_bootcamp.pem -r app/ ubuntu@[Public 4IP]:/home/ubuntu/
8. Enter /home/ubuntu/app in your VM
9. Run node app.js
10 Access your IP:3000 in your browser

## Reverse proxy

1. If you wish to do reverse proxy you must change your default file using `/etc/nginx/sites-available/default` and replace it with:
```
server {
        listen 80;
        server_name _;
        location / {
                proxy_pass http://(public ip):3000;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }
}
```
