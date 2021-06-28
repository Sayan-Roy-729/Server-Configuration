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
```
ssh username@publicIP
```
- Password
- Public / Private Key Pair (Recommended)
- Host Based

## Generating Keys:
```
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
```
cat ~/.ssh/id_rsa.pub | ssh sayan@159.65.152.154 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys"
```
## Copy a file from the local machine to the server:
```
 scp ~/Desktop/move.txt sayan@159.65.152.154:~
 ```
``` 
eval `ssh-agent -s`
ssh-add ~/.ssh/id_rsa_do
```
For windows user, run. (To start ssh-agent, visit [here](https://stackoverflow.com/questions/52113738/starting-ssh-agent-on-windows-10-fails-unable-to-start-ssh-agent-service-erro))
```
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
```
And then add the key
```
ssh-add C:\Users\rsaya\.ssh\id_rsa_do
```
