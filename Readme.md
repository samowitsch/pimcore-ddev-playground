# Pimcore DDEV playground

This repo is a starting point/playground for [Pimcore](https://pimcore.com/de) on an [DDEV Container-based environment](https://www.drud.com/).

## Quick install

> assuming git, ddev, docker and composer is installed on your machine

Tested on:

* Max OS X 10.13.6 High Sierra

```bash
# lets start in your desired folder

# install pimcore
$ COMPOSER_MEMORY_LIMIT=3G composer create-project pimcore/skeleton pimcore-playground

# cd into pimcore-playground folder and run (follow steps. Change only Docroot Location and type in web):
$ ddev config

# modify generated /.ddev/config.yaml to:
# webserver_type: apache-fpm
# router_http_port: "8000"
# router_https_port: "8443"

# copy installer.yaml to /app/config
# copy config_ddev.yml to /app/config
# copy docker-composer.override.yaml to /.ddev

# start up ddev
$ ddev start

# ssh into vm
$ ddev ssh

# go to the parent directory to /var/www/html
cd ..

# run final pimcore install
./vendor/bin/pimcore-install

# your done!
```

Lets start working on your project wiht pimcore ;-)

* Frontend: http://pimcore-playground.ddev.local:8000
* Backend: http://pimcore-playground.ddev.local:8000/admin/login
* phpMyAdmin: http://pimcore-playground.ddev.local:8036


## additional needed config files

### /.ddev/docker-compose.override.yaml

```yaml
version: '3.6'

services:
  db:
    environment:
      # needed for doctrine
      - SERVER_VERSION="5.5"
  web:
    environment:
      - PIMCORE_ENVIRONMENT=ddev
```

### /app/config/config_ddev.yaml

```yaml
imports:
    - { resource: config.yml }
```

### /app/config/installer.yaml

```yaml
# app/config/installer.yml

pimcore_install:
    parameters:
        database_credentials:
            user:                 db
            password:             db
            dbname:               db            
            host:                 db
            port:                 3306
```

## Known issues

* ddev using nginx-fpm is not working properly using apache-fpm instead