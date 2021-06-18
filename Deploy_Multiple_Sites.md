># Deploy Multiple Sites Using Nginx on a VPS:
**Prerequisties:**
- Ubuntu 20.04LTS or 18.04LTS vps
- You have SSH shell access to your VPS with sudo access user.
- Two domain names are pointed to your VPS public IP address. Here I will use two domains *project-purpose.co.in* and *project-backend.in*

>## Install LEMP Stack:
To install LEMP stack, click [here](https://gist.github.com/Sayan-Roy-729/8e0ae624d38e7d8f8d66ec66dee1bfbb)

>## Create Directory Structure
First check the status of Nginx that is there everything is fine or not.
```
sudo systemctl status nginx
```
Best method to host multiple websites is to create a separate document root directory and configuration file for each website. So, you will need to create a directory structure for both websites inside Nginx web root. For best practice, create directory named to your domain names so that you can understand later which directory is for which project.
```
mkdir /var/www/html/project-purpose.co.in
mkdir /var/www/html/project-backend.in
```
Next, you will need to create sample website content for each website. First, create a `index.html` file for project-purpose.co.in website:
```
sudo vim /var/www/html/project-purpose.co.in/index.html
```
Paste the below html code inside this file and save it.
```html
<html>
  <title>project-purpose.co.in</title>
  <h1>Welcome to the project-purpose.co.in with Nginx webserver.</h1>
</html>
```
Now create a `index.html` file for project-backend.in website:
```
sudo vim /var/www/html/project-backend.in/index.html
```
Paste the below html code inside this file and save it.
```html
<html>
  <title>project-backend.in</title>
  <h1>Welcome to the project-backend.in with Nginx webserver.</h1>
</html>
```
Now, have to change the ownership of both directories to `www-data`. Execute below code step-by-step.
```
chown -R www-data:www-data /var/www/html/project-purpose.co.in
chown -R www-data:www-data /var/www/html/project-backend.in
```
>## Create Virtual Configuration:
Next, you will need to create a virtual host configuration file for each website that indicate how the Nginx web server will respond to various domain requests.</br>
First, create a virtual host configuration file for the *project-purpose.co.in* website:
```
sudo vim /etc/nginx/sites-available/project-purpose.co.in
```
Paste below configuration command and save it
```nginx
server {
        listen 80;
        listen [::]:80;
        # Point to the directory for project-purpose.co.in project
        root /var/www/html/project-purpose.co.in;
        index index.html index.htm;
        # Add the domain name here. 
        server_name project-purpose.co.in;

   location / {
       try_files $uri $uri/ =404;
   }

}
```
Now, create a virtual host configuration file for the *project-backend.in* website:
```
sudo vim /etc/nginx/sites-available/project-backend.in
```
Paste the below code and save it.
```nginx
server {
        listen 80;
        listen [::]:80;
        root /var/www/html/web2.webdock.io;
        index index.html index.htm;
        server_name project-backend.in;

   location / {
       try_files $uri $uri/ =404;
   }
}
```
Now enable virtual host with the following command:
```
sudo ln -s /etc/nginx/sites-available/project-purpose.co.in /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/project-backend.in /etc/nginx/sites-enabled/
```
>## Restart the Nginx:
Check Nginx for any syntax error with the following command.
```
sudo nginx -t
```
If everything is fine, then you get success message.
```
sudo systemctl restart nginx
```
>## Test Your Websites:
ow, open your web browser and type the URL http://project-purpose.co.in and http://project-backend.in. You should see both websites with the content we have created earlier

For php back-end sites, click [here](https://webdock.io/en/docs/how-guides/shared-hosting-multiple-websites/how-configure-nginx-to-serve-multiple-websites-single-vps)
