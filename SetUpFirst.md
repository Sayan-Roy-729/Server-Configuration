# First Setup the VPS
**Get $100 credit to create, test and run your vps on digitalOcean from [here](https://m.do.co/c/dbeec3f48f6f)**
## Initial Login:
After creating the VPS, next have to login to that VPS. Here I am using a dummy public IP (18.117.172.192). Simply replace with yours and execute the below command into your local machine terminal. If you are using the AWS EC2, then you have to pass the Key Pair Name otherwise you have to enter the password.
```
ssh root@18.117.172.192
```
Then execute the below code which listed the folder and files that are present in the directory where currently you are.
```
ls
```
And to see the hidden files and folders also execute
```
ls -a
```
Check your parent working directory. This is the directory where currently you are
```
pwd
```
## Create Non-Root User:
If you using the digitalOcean, then you login as a root user. But this is not recommended to use the vps as a root user. So have to create a non-root user first. I am creating a non-root user named *sayan*. You choose what you want. It will ask some questions like password, home addess, phone number. You have to fill this specially the password.
```
adduser sayan
```
After creating the user, you have to give the sudo privilege because when required, you will be change to the root user. Don't give this permissions to the all users.
```
usermod -aG sudo sayan
```

## Firewall Setup:
This is another important things to your vps or droplets. Protect outbound and inbound requests to your vps that creates a security layer to the vps. First you have to allow the ssh, otherwise you first allow the firewall and you don't configure the ssh, then next you can't login. So ssh first
```
ufw allow OpenSSH 
```
Next enable the ufw.
```
ufw enable
```
Now, don't exit from the root terminal. If there is some problems, then you can troubleshoot from here. Now login from another teminal to check that can you login or not.</br>
For more details about firewall visit [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-20-04)
