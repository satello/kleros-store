# Kleros store

## Table of Contents

* [Getting started](#getting-started)
  * [Install dependancies](#install-dependancies)
  * [Database](#database)
      * [Set mongo network](#set-mongo-network)
      * [Run mongo in local](#run-mongo-in-local)
  * [Allow your ip](#allow-your-ip)
  * [Add document](#add-document)
* [Deployment](#deployment)
  * [Redeployment](#redeployment)
* [Set https](#set-https)
* [Api documentation](#api-documentation)


## Getting started

### Install dependancies

```
yarn
```

Run with

```
yarn start
```

GOTO http://localhost:3000.

### Database

#### Set mongo network

If necessary, set the value of `config.database` in `config.js`.

#### Run mongo in local

Assumed mongo is installed

```
sudo mongod
mongo # in another terminal
```

In mongo terminal (JS shell)
```
db.getMongo().getDBNames() # list db
# if `kleros` not exists
use kleros # create `kleros` database
db.getCollectionNames() #list collections
db.klerosDocument.find({}) # list documents
```

### Allow your ip

Add your `ip` in the file `config.js :ipsAllowed`

### Add an document

To add an user you can use the software `compass` and add an entry in the
`kleros` collection.

## API

Kleros store API provides a backend to store the documents
(evidences, contracts).

The api endpoints are describe in `apiDoc`.

### Log requests

All requests are saved in `access.log` file.

## Deployment

### Install node

```
apt-get update
sudo apt-get install nodejs-legacy
sudo apt-get install npm
sudo npm install -g n
sudo n stable
node --version # get at least ths version 8.0.0
sudo ln -sf /usr/local/n/versions/node/<version>/bin/node /usr/bin/node
```

### Install yarn

```
npm install -g yarn
apt install cmdtest
```

### Install pm2

```
npm install pm2 -g
```

### Configuration

Use production configuration :
```
mv bin/www.prod bin/www
pm2 start bin/www # start the server
```

### Redeployment

```
pm2 stop www # or id like 0
git pull
pm2 start bin/www
```

### Set up database

Set database in `config.js`.

## Set https

```
# Install tools that Let’s Encrypt requires
sudo apt-get install bc

# Clone the Let’s Encrypt repository to your server
sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt # or sudo apt-get install letsencrypt

/opt/letsencrypt/certbot-auto renew

dig +short kleros-store.com
# output should be your droplet’s IP address, e.g. 138.68.11.65

# Move into the Let’s Encrypt directory
cd /opt/letsencrypt

# Create the SSL certificate
./certbot-auto certonly --standalone

/opt/letsencrypt/certbot-auto renew

sudo crontab -e

00 1 * * 1 /opt/letsencrypt/certbot-auto renew >> /var/log/letsencrypt-renewal.log
```

## Api documentation

### Generate api documentation

```
yarn add global apidoc
apidoc -f "controllers/.*\\.js$" -i ./  -o apidoc/ # bug with fish terminal (use bash)
```

### Go to api documentation

```
open http://localhost:3000/apidoc/
```

### Regenerate api documentation

```
rm -rf apidoc
apidoc -f "routes/.*\\.js$" -i ./  -o apidoc/ # bug with fish terminal (use bash)
```
