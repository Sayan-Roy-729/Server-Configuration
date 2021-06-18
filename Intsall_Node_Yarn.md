># Install Node, npm and Yarn on VPS:

>## Update pre-intstalled packages:
```
sudo apt update
```

>## Install `curl`:
```
sudo apt-get install curl
```

>## Install `Node` & `npm`:
Enable Node source (Execute 1st command for latest version or 2nd for lts version)
```
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```
OR
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```
Install node
```
sudo apt-get install nodejs
```
Now Check installed *node* and *npm* version.

>## Install 'yarn':
Add GPG Key
```
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
```
Enable Yarn Repository
```
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```
Update Cache & Install Yarn
```
sudo apt update && sudo apt install yarn
```
Now check the installed yarn package and that's it.
