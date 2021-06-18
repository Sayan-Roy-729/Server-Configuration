># React.JS APP Deployment on VPS Using Nginx
>## Update The pre-installed packages:
```
sudo apt update
```

>## INSTALL `curl` package:
```
sudo apt-get install curl 
```

>## Install `Node.JS` & `npm`:
- Enable Node source (**Execute 1st command for latest version or 2nd for lts version**)
```
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```
**OR**

```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```

- Install node and npm
```
sudo apt-get install nodejs
```
Then check installed node version by `node -v` and `npm -v`

>## Install `Nginx`:
```
sudo apt-get install nginx
```
Check the installed version
```
nginx -v
```
Now if you browse with the public IP of the VPS, you can see the default static nginx page. Now enable Nginx so that it automatically starts if the server restarts.
```
sudo systemctl enable nginx
```
You also can check the status of nginx
```
sudo systemctl status nginx
```
>## Get Project into the Server:
You can upload the project files through *remote server git* or *by cloning the github repo* or *upload files directly with the help of Cyberduck or Filezilla.* You can also build the react app into the server and then deploy that or also you can build the react app into your local machine and then upload that build files into server.</br>
Create a folder inside your *home/username* directory and clone from your github repository.</br>
The best thing about react application is that, after Build the project, it is similar to the static websites created with just vanilla javascript. So can use that to the nginx static website.


>## Copy Build Folder project into var/www/html:
Now copy the contents of the build folder into var/www/html. It’s optional. you can put your files anywhere you like. you can create your own separate folder for each project and keep the generated assets there. For simplicity, we are keeping them under var/www/html/build. So it will be our root.
```
sudo cp -r /home/ubuntu/your-project-folder/build/. /var/www/html
```

>## Configure NGINX to serve static files
- Open default site provider by the nginx to your editor. Here I am using vim.
```
sudo vim /etc/nginx/sites-available/default
```
Now change the commands of the file. Final result will look like this
```nginx
server {
  # Listen the http request coming from the user client
  listen 80 default_server;
  listen [::]:80 default_server;
  # Directory where the files this server can get
  root /var/www/html;
   # Tell server which file is the root file to serve first
  index index.html index.htm index.nginx-debian.html;
   # You can add your domain name here
  server_name <Your Public IP>;
  # inside location we can serve our static contents like image or pdf etc.
  location / {
    try_files $uri $uri/ =404;
  }
}
```
Now save this file by pressing `:wq`. We changed the default configuration of nginx. To test that the changes is successful run below command
```
sudo nginx -t
```
If there is successful message then go further. Otherwise check and find-out the error. Restart the nginx.
```
sudo service nginx restart
```
Now you can browser your public server's public IP address and you can see the website.

>## Configure Nginx as a Proxy:
You can deploy the react application without doing build process. E.g. If you run the react app as the development time. If you want to host like that, then have to configure a little different way because user will never search like *http://yourPublicIP:3000*. 3000 is the port number. SO we will receive the *http://yourPublicIP* and then redirect to the port. To start deployment, first download the dependencies via npm or yarn</br>
First open the default configuration file of nginx
```
sudo vim /etc/nginx/sites-available/default
```
After changing the commands, the final look should like this
```nginx
server {
  # Listen the http request coming from the user client
  listen 80 default_server;
  listen [::]:80 default_server;
  # Directory where the files this server can get
  root /var/www/html;
   # Tell server which file is the root file to serve first
  index index.html index.htm index.nginx-debian.html;
   # You can add your domain name here
  server_name <Your Public IP>;
  # inside location we can serve our static contents like image or pdf etc.
  location / {
    proxy_pass http://localhost:3000
    try_files $uri $uri/ =404;
  }
}
```

>## Install `PM2`:
If you want to leave like this, then there will be problem because you run your project with the help of `npm` or `yarn`. Then this will run in development mode. So if there will some users visit your site, it will crash. To run and automatically restart the application inside your VPS, have to install `pm2` or `forever`. Here I will show `pm2`. Install that first
```
sudo npm i pm2 -g
```
Start the pm2. 
```
pm2 start node_modules/react-scripts/scripts/start.js --name "your-project-name"
```
Other comments offered by `pm2`:
```
# Other pm2 commands
pm2 show app
pm2 status
pm2 restart app
pm2 stop app
pm2 logs (Show log stream)
pm2 flush (Clear logs)

# To make sure app starts when reboot
pm2 startup ubuntu
```

>## Enable the `firewall`:
This is the security layer to your VPS. You can allow which can enter and which can go.

```
sudo ufw allow ssh // Allow to connect to your vps with ssh.
sudo ufw enable // Enable the UFW Firewall
sudo ufw allow http // Allow http port
sudo ufw allow https // Allow https connection
sudo ufw allow 'Nginx Full'  // to allow full nginx
sudo ufw allow 21 // If you using Cyberduck or filezilla then run this command
```
For more defails about firewall, click [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-20-04)

>## Adding SSL Certificate with LetsEncrypt:
If there is no green lock of your website, then users do not want to visit the site. You can enable without any cost. To enable this, fist add `A Record` to your domain. You can do it by going to the domain provider's dashboard from where you can buy the domain. Add the vps local ip as a A Record and then execute the following commands.
```
sudo apt-add-repository -r ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```
It will ask for your email and you are done. This certificate will be valid for 90 days. You have to run the following command to update your certificate.
```js
certbot renew --dry-run
```
