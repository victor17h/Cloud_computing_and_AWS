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
10. Access your IP:3000 in your browser

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
2. Then do `sudo systemctl restart nginx`
3. `systemctl status nginx` to check if its running
4. Access your browser with your public instance ip:3000

## Mongodb

1. Create an instance for mongodb on AWS
2. During the creation add port 27017 and on custom ip introduce the public ip from the other instance with the end /ip from your private ip.
3. Access your virtual machine on your shell using `ssh -i "devop_bootcamp.pem" user@awsVirtualMachineLink.com` provided by the instance created.
4. Export local db folder from ~/.ssh to your virtual machine using `scp -i devop_bootcamp.pem -r app/ ubuntu@[Public 4IP]:/home/ubuntu/`.
5. Run the provision file you just exported in your virtual machine using `./provision.sh` if it does not work try `ls` and if provision file is not in green color you will need to give permissions by using `chmod +x provision.sh`. Then try again running `./provision.sh`.
6. Once everything is installed, access cd /etc
7. Next, do sudo nano mongod.conf to edit the file and use the following code:
```
storage:
  dbPath: /var/lib/mongodb
  journal:
    enabled: true
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log
net:
  port: 27017
  bindIp: 0.0.0.0
```
8. Then, do `sudo systemctl restart mongod` to apply the changes.
9. After that, `sudo systemctl enable mongod`.
10. Check status using, `systemctl status mongod` and see if its running.

## App config

1. After setting app and reverse proxy, you will need to `sudo systemctl restart nginx` and check status using `systemctl status nginx` to see if nginx is still running.
2. Insert the environment variable using `sudo echo "export DB_HOST=mongodb://(dbip):27017/posts" >> ~/.bashrc` inserting the mongodb instance public ip and finally use `source ~/.bashrc`.
3. cd into app
4. Then, `cd seeds`.
5. Use `node seed.js`.
6. cd again into app
7. Finally use `npm start`
8. If the process is done correctly, you would be able to enter your app instance public ip on the browser.
9. Try also app public ip/posts to see if the process has been completed successfully.

## AWS VPC

1. Create VPC CDIR block 10.0.0.0/16
2. Create internet gateway and attach to our VPC
3. Create public subnet within our VPC, 10.0.1.0/24
4. Create private subnet within our VPC, 10.0.2.0/24
5. Click on route tables and select table - edit, routes -target - IG that was created
6. Associate public subnet with our route table - click on subnet association, then - edit subnet association - select public subnet we created earlier.
- Diagram![image](https://user-images.githubusercontent.com/74541774/122251331-503bb680-cec2-11eb-96ea-9c3a2ce99890.png)

