# Deploy Node App Using Nginx:
**💡You can deploy any node app (template engine app or api app) into your server.**
>## 🎆 Update Pre-installed packages:
```
sudo apt update 
```

>## 🎆 Install `curl`:
```
sudo apt-get install curl 
```
>## 🎆 Install ` node.js`:
Enable Node source (Execute 1st command for latest version or 2nd for lts version)
```
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
OR
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```
Install Node
```
sudo apt-get install nodejs
```

>## 🎆 Install `Nginx`:
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

>## 🎆 Get Project into server:
Next, You have to upload the project files into the server. For that you can use git server remote or clone from github or can directltly upload via Cyberduck or FileZilla softwares.

>## 🎆 Install Dependencies & Test App:
Navigator to your project directory and execute below code. If you are using yarn, you can do also.
```
npm install 
```
Start your app. To start see your `package.json` to see your start command
```
npm start
```
If the app run successfully, stop the app.

>## 🎆 Install `pm2`:
To keep your app running all time and to restart automaticlly if the app is crashed, pm2 is required.
```
sudo npm i pm2 -g
```
Now inside your root project directory execute below code
```
pm2 start <your root file name> --name nodeApp
```
*--name nodeApp* will give this process a name that helps to identify thw project.
Other *pm2* Command:
```
pm2 show app
pm2 status
pm2 restart app
pm2 stop app
pm2 logs (Show log stream)
pm2 flush (Clear logs)

# To make sure app starts when reboot
pm2 startup ubuntu
```
**You should now be able to access your app using your IP and port.**

>## 🎆 Setup `ufw` firewall:
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

>## 🎆 Configure `nginx`:
Open the default configuration file of nginx with your terminal text editor
```
sudo vim /etc/nginx/sites-available/default
```
Add the following to the location part of the server block
```
    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://localhost:5000; #whatever port your app runs on
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
```
Check NGINX config
```
sudo nginx -t
```
Restart nginx
```
sudo service nginx restart
```

>## 🎆 Add SSL with LetsEncrypt
Execute below commands step-by-step
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```
This SSL Certificate is valid only for 90 days. You can renew by below command.
```
certbot renew --dry-run
```
**Now visit your domain and can see the sites.**
