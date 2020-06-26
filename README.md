# Host a Flask app on an EC2 Instance
## Setting up an EC2 instance

1. From your AWS Dashboard go to EC2 and launch an instance. 

2. Choose your preferred Amazon Machine Image (AMI). My preferred AMI is the Ubuntu Server. 

3. Go ahead and choose an instance type. I usually use t2.micro for simple projects, so select an instance type based on your requirements. 

4. Continue through to "Configure Security Group" without making any modifications. Security Groups is the most important part because without proper configuration of incoming and outgoign traffic you may or may not be able to access your web app. Select "All traffic" in Type and allow Source to be "Anywhere". 

5. Next, go ahead and finish launcing your instance. For good security measures you want to have better inbound and outbound security group rules, but since this guide is for tutorial purposes I'm good with the security rules. 

6. Choose your key pair before finishing(If you don't already have one you'll have an option to create one).

Congratulations, you've successfully created and launched an EC2 instance!

Connect to your running instance by clicking on it and selecting "Connect" at the top. Copy the ssh command under "Example" and paste it in your terminal. Your SSH command won't work unless you're in the same directory as your .pem file. 

Now you should be connected to your instance via the command line/terminal.


## Preparing the Instance to Host the App

First thing's first. We need to install git, pyhton and pip for us to continue with our project.

#### Start by updating the package index:

```
sudo apt update
```

#### Install git:
```
sudo apt install git
```

#### Install Python 3.8:
```
sudo apt install python3.8
```

#### Check the version of Python installed:
```
python --version
```

#### Install pip3:
```
sudo apt-get install python3-pip3
```


#### Clone the github repo for your Flask app in the virtual machine/EC2 Instance:
```
git clone [your repo link]
```

#### cd to your new directory:
```
cd [directory name]
```

#### Install your requirements.txt file:
```
pip3 install -r requirements.txt
```

More about requirements.txt file [here](https://medium.com/@boscacci/why-and-how-to-make-a-requirements-txt-f329c685181e)

(just include the required packages in your .txt file if you're constantly hit by errors while installing the dependencies)



###### Don't worry if you don't have a python project created. Create a simple python file using vim:
```
mkdir proj

cd proj

touch app.py

vi app.py
```
###### once in Vim, hit "i" to enter "insert mode" and paste this:
```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return "The App is running!"

if __name__ == '__main__':
    app.run(debug=True)
```

###### press the esc key hit ":" then "x" and "enter" key.




#### We'll run the Flask app in a virtual environment, so lets install virtualenv:
```
sudo pip3 install virtualenvwrapper

sudo su apps

vi .bashrc

```
#### Add the following:
```
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS='--no-site-packages'

source /usr/bin/virtualenvwrapper.sh
```

#### Make a virtual environment:
```
mkvirtualenv myapp
```

#### Install gunicorn in your virtual environment:
```
pip3 install gunicorn

exit
```

#### Next up, we need to install a web server on our VM:
```
sudo apt-get install nginx

sudo vi /etc/nginx/nginx.conf
```

##### replace this line:
```
user  nginx;
```
##### with:
```
user  apps;
```

##### in the http block add this:
```
server_names_hash_bucket_size 128;
```

#### Define a server block for our site:
```
sudo vi /etc/nginx/conf.d/virtual.conf`
```

##### paste this inside:
```
server {
    listen       80;
    server_name  your_public_dnsname_here;

    location / {
        proxy_pass http://127.0.0.1:8000;
    }
}
```

#### Start the server:
```
sudo /etc/rc.d/init.d/nginx start
```

#### Start the Gunicorn process to serve our Flask app:
```
sudo su apps

gunicorn app:app -b localhost:8000 &
```

The app should be up and running.

Go back to your EC2 instance on your browser, copy your EC2's **Public DNS(IPv4)** address and paste it in a new tab. If successful, the browser will display the message "The App is running!"

***Congratulations*** you've successfully deploye dyour Flask app on an EC2 Instance!


