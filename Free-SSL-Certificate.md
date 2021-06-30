# Enable SSL Certificate For Free:
**Get $100 credit to create, test and run your vps on digitalOcean from [here](https://m.do.co/c/dbeec3f48f6f)**</br>
Get Free SSL certificate for your website without any cost. For that, follow the steps.
## Install `snapd`:
You'll need to install snapd
```
sudo apt install snapd
```
For more instruction, click [here](https://snapcraft.io/docs/installing-snapd)
## Ensure that your version of snapd is up to date:
Execute the following instructions on the command line on the machine to ensure that you have the latest version of snapd. 
```
sudo snap install core; sudo snap refresh core
```

## Remove certbot-auto and any Certbot OS packages:
If you have any Certbot packages installed using an OS package manager like apt, dnf, or yum, you should remove them before installing the Certbot snap to ensure that when you run the command certbot the snap is used rather than the installation from your OS package manager. Exact command is
```
sudo apt-get remove certbot, sudo dnf remove certbot
```
If you previously used Certbot through the certbot-auto script, you should also remove its installation by following the instructions [here](https://certbot.eff.org/docs/uninstall.html)

## Install Certbot:
Run this command on the command line on the machine to install Certbot.
```
sudo snap install --classic certbot
```

## Prepare the Certbot command:
Execute the following instruction on the command line on the machine to ensure that the certbot command can be run. You will get error if the required file is already exists. otherwise it will prepare the certbot.
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

## Get & Install the Certificate:
Execute the below command.
```
sudo certbot --nginx
```
If you get any error like *Could not choose appropriate plugin: The requested nginx plugin does not appear to be installed* or something like that, then execute the below command and run again the above command
```
sudo apt-get install python3-certbot-nginx
```

## Test automatic renewal:
 The Certbot packages on your system come with a cron job or systemd timer that will renew your certificates automatically before they expire. You will not need to run Certbot again, unless you change your configuration.</br>
You can test automatic renewal for your certificates by running this command
```
sudo certbot renew --dry-run
```
If that command completes without errors, your certificates will renew automatically in the background. 

## Confirm that Certbot worked:
To confirm that your site is set up properly, visit the site in your browser and look for the lock icon in the URL bar.
