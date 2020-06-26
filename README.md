# Host a Flask app on an EC2 Instance
## Setting up an EC2 instance

From your AWS Dashboard go to EC2 and launch an instance. Choose your preferred Amazon Machine Image (AMI). My preferred AMI is the Ubuntu Server. Go ahead and choose an instance type. I usually use t2.micro for simple projects, so select an instance type based on your requirements. Continue through to "Configure Security Group" without making any modifications. Security Groups is the most important part because without proper configuration of incoming and outgoign traffic you may or may not be able to access your web app. Select "All traffic" in Type and allow Source to be "Anywhere". Next, go ahead and finish launcing your instance. For good security measures you want to have better inbound and outbound security group rules, but since this guide is for tutorial purposes I'm good with the security rules. Choose your key pair before finishing(If you don't already have one you'll have an option to create one). Congratulations, you've successfully created and launched an EC2 instance!

Connect to your running instance by clicking on it and selecting "Connect" at the top. copy the ssh command under example and paste it in your terminal. Your SSH command won't work unless you're in the same directory as your .pem file. You should be connected to your instance via the command line.


## Preparing the Instance to Host the App

