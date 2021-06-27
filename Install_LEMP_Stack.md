># Install LEMP Stack into your VPS
**Get $100 credit to create, test and run your vps on digitalOcean from [here](https://m.do.co/c/dbeec3f48f6f)**
**Prerequisties:**
- Ubuntu 20.04LTS or 18.04LTS vps
- You have SSH shell access to your VPS with sudo access user

>## First update the pre-installed packages
```
sudo apt update
```
>## Install Nginx
```
sudo apt install nginx
```
Now browse ypur public IP into your browser and can see a default static nginx site. That's mean nginx is installed correctly. [NOTE: If you are using a AWS EC2 instance, then you have to allow port 80 from the security group.]
- Install MySQL
```
sudo apt install mysql-server
```
>## Install PHP
```
sudo apt install php-fpm php-mysql
```
Now LEMP Stack is installed into your VPS.

>## Configure the Nginx:
Nginx does not support php by-default. So to run php have to configure little more. </br>
Have to create a `index.php` file inside */var/www/html* directory. I am using vim editor. Below comman will create the file.
```
sudo vim /var/www/html/index.php
```
Paste the below php code inside the file.
```php
<?php
    php.phpinfo();
 ?>
```
Now Open default Nginx configuration file.
```
sudo vim /etc/nginx/sites-available/default
```
Change the file to the below commands and save it.
```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /var/www/html;
    index index.php index.html index.htm index.ngimx-debian.html;
    server_name _;
    location / {
        try_files $uri $uri/ =404;
    }
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        include fastcgi_params;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

>## Restart the Nginx:
First, check is everything is ok or not. For that execute the below command. It will test the nginx configuration
```
sudo nginx -t
```
If the messages is successful, then restart the nginx:
```
sudo systemctl restart nginx
```
or
```
sudo service nginx restart
```
Then check the status of the nginx
```
sudo systemctl status nginx
```
Now enable nginx so that it automatically starts if the server restarts.
```
sudo systemctl enable nginx
```
Check the nginx status
```
sudo systemctl status nginx
```

Now browse again the IP address and can see interseting things.
