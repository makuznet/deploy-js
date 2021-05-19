# Deploying json.org

> This repo is a nodejs.org deployment report.    

## Defenitions
Nodejs.org — web site JavaScript source code for training.
NVM (nvm) — Node.js Version Manager.  
Node.js — JavaScript interpreter framework. 
NPM (npm) — Node.js Packet Manager (installed with Node.js).  

## List of commands 
```shell
terraform plan
terraform apply --auto-approve
ssh makuznet-at-gmail-com-nodejs.devops.rebrain.srwx.net -l root 
    apt update
    apt -y install nginx
    apt -y install curl
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
    exit
ssh makuznet-at-gmail-com-nodejs.devops.rebrain.srwx.net -l root
    nvm --version
    apt -y install git
    mkdir /var/www/nodejs.org
    cd /var/www/nodejs.org
        git clone https://github.com/nodejs/nodejs.org .
        nvm install v14.16.1
        npm install
        npm run build
        cd ~
    apt -y install letsencrypt
    letsencrypt certonly --webroot -w /var/www/html -d makuznet-at-gmail-com-nodejs.devops.rebrain.srwx.net -m makuznet@gmail.com --agree-tos
    vi /etc/nginx/nginx.conf
    rm -i /etc/nginx/sites-enabled/default
    systemctl reload nginx
    exit
scp -r root@makuznet-at-gmail-com-nodejs.devops.rebrain.srwx.net:/etc/letsencrypt ~/Documents/rebrain/deploy-js/        
```

## Usage 
### Nginx config file
Follow `Installation` section and then `List of commands` section to deploy nodejs.org.  

### When cloning a JS app to your project dir
> Your project dir must be empty without git initiated!  
> Don't miss the dot at the end of the command !  
```bash
git clone https://github.com/nodejs/nodejs.org .
```

## Installation   
### NVM installation
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
nvm --version
```
### Node.js installation
```bash
nvm ls-remote # node.js version list
nvm install v14.16.1 
nvm list # installed node.js list
node -v # current node.js version
nvm use v14.16.1 # use a particular version if there are many installed
```
### nodejs.org installation
```bash
git clone https://github.com/nodejs/nodejs.org .
npm install
npm run build
apt install nginx
vi /etc/nginx/sites-enabled/default
    server {
        listen 80 default_server;
        
        root /Users/makuznet/Documents/rebrain/deploy-js/nodejs.org/build/en/;
        
        index index.html index.htm index.nginx-debian.html

        server_name _;

        location / {
            try_files $uri $uri/ =404
        }
    }
nginx -t
systemctl restart nginx
```
### Letsencrypt
```bash
sudo apt install letsencrypt

# getting an ssl certificate
letsencrypt certonly --webroot -w /var/www/html -d makuznet-at-gmail-com-nodejs.devops.rebrain.srwx.net -m makuznet@gmail.com --agree-tos

# backing up /etc/letsencrypt
scp -r root@makuznet-at-gmail-com-nodejs.devops.rebrain.srwx.net:/etc/letsencrypt ~/Documents/rebrain/deploy-js/
```

### Extra
#### Installing backend server nodejs-against-humanity
```bash
nvm list
nvm use v8.1.0 # change node.js version
cd /your_project/nodejs-against-humanity
git clone https://github.com/amirrajan/nodejs-against-humanity .
npm install # install all the packages and dependencies listed in the package.json file
npm run start # start is the alias for a script; located in the package.json file
```

#### Demonization of a backend server
```bash
npm install -g pm2
pm2 -n game -i 1 start server.js
pm2 logs
pm2 list
pm2 monit
```
-n — name  
-i — instance  

#### Yarn — alternative node.js package manager
```bash
npm install -g yarn
```

## Acknowledgments

This repo was inspired by [rebrainme.com](https://rebrainme.com) team

## See Also
- [nvm](https://github.com/nvm-sh/nvm)
- [Installing Node.js with nvm](https://gist.github.com/d2s/372b5943bce17b964a79)
- [nodejs.org](https://github.com/nodejs/nodejs.org)
- [Node.js website setup](https://github.com/nodejs/build/tree/master/ansible/www-standalone)

## License
Follow all involved parties licenses terms and conditions.