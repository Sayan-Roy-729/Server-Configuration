# Password Less Login:
**Get $100 credit to create, test and run your vps on digitalOcean from [here](https://m.do.co/c/dbeec3f48f6f)**</br></br>
You manage the servers and you have to login for dirrerent vps with different password. If there is any system that you don't have to enter the password and you also login securly, then is not it helpful? 
## Create a Public Private Key Pair:
Execute the below command in your terminal and simply go forward by only pressing enter key from keyboard.
```
ssh-keygen -t rsa
```

## Deploy SSH KEY on your Server:
Login to the VPS server and run the following commands.
```
cd ~
mkdir .ssh
```
Now exit from the server terminal and execute the below code. Replace the *userName* to yours that is defined in your local computer and change server user name (for my case *sayan*) and public IP address(for my case *165.232.188.124*) according to yours. Then hit enter and have to type the password. The below code is for windows user. For mac and linux users, follow [here](https://docs.digitalocean.com/products/droplets/how-to/add-ssh-keys/to-existing-droplet/)
```
scp C:\Users\userName\.ssh\id_rsa.pub sayan@165.232.188.124:~/.ssh/authorized_key
```
After the successful execution of the above code, you can login without entering the password.

## Test Time:
Now enter the below command by changing the required username and IP address and you should login to the vps.
```
ssh sayan@165.232.188.124
```

## Quick Login Using Powershell Profiles (For windows users):
### Create a powershell profile:
Execute the below command
```
New-Item $profile -Type File -Force
```

## Open and add a function to the PowerShell profile:
Execute the command.
```
notepad $profile
```
This will open a notepad editor and add the following commands to the notepad. You can add functions as many as you want. Only change the username and the IP address. Then save the notepad and close it. ALso close the powershell. 
```
echo "Hello Sayan, Welcome to PowerShell. Your profile works!"

function personal{
    Start-Process ssh sayan@165.232.188.124
}

function client1{
    Start-Process ssh user@139.39.45.126
}

function client2{
    Start-Process ssh root@239.59.45.126
}
```
Reopen the powershell and type only *personal*(If you don't restart the powershell, it will not work). You can see that you are logged in the VPS. 
