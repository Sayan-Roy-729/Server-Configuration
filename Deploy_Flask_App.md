># Deploy Flask App on VPS:
>## Install required packages:
Update preinstalled packages
```
sudo apt update 
```
Now install python3, python3-pip & Nginx
```
sudo apt install python3-pip python3-dev nginx
```

>## Create a directory for flask app and virtual environment:
Lets install virtual environment
```
sudo pip3 install virtualenv
```
Lets create a directory to host our flask application
```
mkdir myFlaskApp && cd myFlaskApp
```
Run the following command to create a virtual environment named *env*
```
virtualenv env
```
Finally activate the virtual environment
```
source env/bin/activate
```
Now install flask and gunicorn using flask
```
pip3 install flask gunicorn
```

>## Create a sample project & wsgi entry point:
Let us now create a sample project by entering the command below:
```
vim app.py
```
Paste the below code to *app.py* file. Here *debug = true* is written. For the first time to deploy the app, then this is for testing purpose. Then make it false. This is actually for development purpose.
```python
from flask import Flask
app = Flask(__name__)
@app.route('/')
def hellow_world():
    return "Hello WOrld!"
if __name__ == '__main__':
    app.run(debug = True, host = '0.0.0.0')
```
Now run the project. If everything is ok, then go further. </br>
Next, we will create a file that will serve as the entry point for our application. This will tell our Gunicorn server how to interact with the application.
```
vim wsgi.py
```
Copy the contents below to *wsgi.py*
```python
from app import app

if __name__ == "__main__":
    app.run()
```
We can test gunicorn's ability to serve our project by running the command below
```
gunicorn --bind 0.0.0.0:5000 wsgi:app
```
Folder structure so far:
```
myFlaskApp
  |____ app.py
  |____ wsgi.py
  |____ env
```
Lets deactivate the virtual environment now
```
deactivate
```

>## Creating a systemd service:
When your server will reboot for any reason then the flask app will rerun automatically. Lets now create a systemd service using:
```
vim /etc/systemd/system/app.service
```
Now paste the contents below to this file and save it:
```
[Unit]
#  specifies metadata and dependencies
Description=Gunicorn instance to serve myproject
After=network.target
# tells the init system to only start this after the networking target has been reached
# We will give our regular user account ownership of the process since it owns all of the relevant files
[Service]
# Service specify the user and group under which our process will run.
User=sayan
# give group ownership to the www-data group so that Nginx can communicate easily with the Gunicorn processes.
Group=www-data
# We'll then map out the working directory and set the PATH environmental variable so that the init system knows where our the executables for the process are located (within our virtual environment).
WorkingDirectory=/home/sayan/myFlaskApp/
Environment="PATH=/home/sayan/myFlaskApp/env/bin"
# We'll then specify the commanded to start the service
ExecStart=/home/sayan/myFlaskApp/env/bin/gunicorn --workers 3 --bind unix:app.sock -m 007 wsgi:app
# This will tell systemd what to link this service to if we enable it to start at boot. We want this service to start when the regular multi-user system is up and running:
[Install]
WantedBy=multi-user.target
```
[**NOTE: Change the username according to you. Change the *WorkingDirectory* according to your project and also specify the *Environment* path.**] </br>
Activate this service by typing
```
sudo systemctl start app
sudo systemctl enable app
```
A file named *app.sock* will be automatically created. If there is no .sock file, you did a mistake. Folder structure so far
```
myFlaskApp
  |____ app.py
  |____ wsgi.py
  |____ env
  |____ app.sock
```

>## Configure Naginx
Create a file named app inside /etc/nginx/sites-available
```
sudo vim /etc/nginx/sites-available/app
```
Now copy the below contents to this file:
```nginx
server {
    listen 80;
    server_name 165.232.177.116; # This is your local ip address

    location / {
        include proxy_params;
        # tell where the files of the site
        proxy_pass http://unix:/home/satan/myFlaskApp/app.sock;
    }
}                
```
Activate this configuration by executing this
```
sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled
```
Restart nginx and your website should work fine!
```
sudo systemctl restart nginx
```
>## Firewall Setup
Execute below commands
```
sudo ufw allow ssh
sudo ufw enable
sudo ufw allow 'Nginx Full'
sudo ufw allow http
sudo ufw allow https
```

>## What to do if you changes sometimg to your project?
After changing the project, next have to execute below commands
```
sudo service nginx restart
sudo service app restart
```

>## How to serve static files?
In you project, there will be static files also. To serve that files also, type these commands:
```
sudo vim /etc/nginx/sites-availabe/app
```
Next have to change the file. After changing the file, the file should look like this
```
server {
    listen 80;
    server_name 165.232.177.116;
    
    location /static {
        alias /home/sayan/myFlaskApp/static/;
    }
    
    location / {
        include proxy_params;
        proxy_pass http://unix:/home/sayan/myFlaskApp/app.sock;
    }
}
```
