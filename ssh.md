# SSH Details:
## What is SSH?
- Secure Shell
- Communication Protocol (like http, https, ftp etc)
- Do just about anything on the remote computer
- Traffic is entrypted
- Used mostly in the terminal/command line

## Client / Server Communication
- SSH is the client
- SSHD is the server (Open SSH Daemon)
- The server mush have sshd installed and running or you will not be able to connect using SSH

## Authentication Methods:
```bash
ssh username@publicIP
```
- Password
- Public / Private Key Pair (Recommended)
- Host Based

## Generating Keys:
```bash
ssh-keygen -t rsa
```
- ~/.ssh/id_rsa (Private Key)
- ~/.ssh/id_rsa.pub (Public Key)
- Public key goes into the server *authorized_keys* file

## What about Windows?
- Windows 10 now supports native SSH
- Putty is used in older versions of Windows
- Git Bash & other terminal programs include the ssh command & other Unix tools

## Add SSH Key to the server:
```bash
cat ~/.ssh/id_rsa.pub | ssh sayan@159.65.152.154 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys"
```
## Copy a file from the local machine to the server:
```bash
 scp ~/Desktop/move.txt sayan@159.65.152.154:~
 ```
 ## DigitalOcean:
 Create an account in DigitalOcean. If you create an account from [here](https://m.do.co/c/dbeec3f48f6f) then you will get $100 creadit for 2 months that you can use whatever you need.
 
 ### Create Keys for Droplets (id_rsa_do):
 Before creating any droplet, create a ssh key for the digital ocean (I like to use different keys for different use case. For that change the name of the file like *id_rsa_do* do for digitalOcean).
 ```bash
 ssh-keygen -t rsa
 ```
 ### Create a Droplet:
Now time to create a droplet. When creaing that, choose the ssh key instead choosing the password authentication. Now execute below code in your local machine terminal.
```bash
cat ~/.ssh/id_rsa_do.pub
```
You can see your public key. Copy the key (NOTE: Copy carefully, if there is any white space at the end and start of the key, then you can't login to your droplet) and paste this into the ssh key input form of the digitalOcean. Then save and create the final droplet. 
Could not open a connection to your authentication agent.
### Login To Your Droplet:
Now have to login to your droplet from your local machine. I am using *43.110.244.47* as a public IP. You replace with yours.
```bash
ssh root@143.110.244.47
```
If you get *Prmission denied (Publickey)* message, then you have to add the *id_rsa_do* ssh key to your machine. For that execute below code.
```bash
ssh-add ~/.ssh/id_rsa_do
```
If you get another error message like *Could not open a connection to your authentication agent* then you have to execute below command to activate the ssh agent and then execute the above command.
```bash
eval `ssh-agent -s
```
For windows users run as an administrator powershell to start the ssh agent.
```
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
```bash
And then add the key by replacing the *sayan* to the user name defined to your local machine.
```
ssh-add C:\Users\sayan\id_rsa_do
```bash
After that, try to login again to the droplet. Now you should login.
```bash
ssh root@143.110.244.47
```
### Update packages:
Update the packages that are already installed.
```bash
sudo apt update
sudo apt upgrade
```
### Create a new Non-Root User WIth Sudo Previlege:
You should not use and login next times as a *root* user because this user has maximum power to control your vps. So first create a new user. Here, I am creating a user named *sayan*. You can give a any name. When you are crearting the user, you are asked to enter password and other details.
```bash
adduser sayan
```
Check the info of the user.
```bash
id sayan
```
Give the sudo privilege to this user.
```bash
usermod -aG sudo sayan
```bash
Again check the info of the user. After executing the below command, you can see that this user got the sudo access.
```bash
id sayan
```
### Login as Non-Root User:
Now open another terminal and try to login as non-root user.
```bash
ssh sayan@143.110.244.47
```
You will get *Permission denied* error because you add the ssh key for the root user. You also have to add the key for this user also. For that, login as root user for the last time. Create an directory named *.ssh* inside the user home directory and navigate to the directory
```bash
mkdir /home/sayan/.ssh && cd .ssh
```
Now create a file named `authorized_keys`
```bash
touch authorized_keys
```
Now open the file with the help of the terminal editor like *vim* or *nano*.
```bash
sudo vim authorized_keys
```
Here paste the key of `id_rsa_do.pub` file (If you can't get the public key from your local machine, then see the above process where you paste the key to create the droplet.) Now again try to login with non-root user to your vps.
```bash
ssh sayan@143.110.244.47
```
### Disable root password login:
It is recommended to disable this for security reason. For that, first login as non-root user and execute the below command to open the *sshd* configuration file.
```bash
sudo vim /etc/ssh/sshd_config
```
Fine the required lines and set the followings. To know what it changes, read from [here](https://askubuntu.com/questions/449364/what-does-without-password-mean-in-sshd-config-file) for root login.
```bash
PermitRootLogin no
PasswordAuthentication no
```
Save the file and close. Now reload the *sshd*
```bash
sudo systemctl reload sshd
```
### hange the Owner of /home/sayan/* to sayan:
Execute the below code to see that which folders can only access by root user.
```bash
ls -la
```
You can see that the folder *.ssh* can only access the root user. But the root user can't login any more. So execute below code to change the ownership of the folder.
```bash
sudo chown -R sayan:sayan /home/sayan
```
May need to set permission.
```bash
chmod 700 /home/sayan/.ssh
```
### Github:
To access the github, also have to create a public-private key into your server. Name *id_rsa_github* like previous time.
```bash
ssh-keygen -t rsa
```
And then public key (id_rsa_github.pub) to your github account. Now clone a repository from your github and can deploy your code. To deploy the projects, you can visit my other files.
