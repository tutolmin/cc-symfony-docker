# cc-symfony-docker
Symfony replica for local development

Check versions (it is proved to work with the following versions)

**Docker version 19.03.6**

**docker-compose version 1.17.1**

**git version 2.17.1**

Prepare environment for the project

```
mkdir ~/chesscheat; cd ~/chesscheat

git clone git@github.com:tutolmin/cc-symfony-docker.git

cd cc-symfony-docker

newgrp docker
```

Add host alias for the project in your **/etc/hosts** file
```
127.0.1.1	chesscheat.local
```

Get local copy of the project (particular branch)
```
cd apps

git clone --single-branch --branch new_layout git@github.com:tutolmin/symfony.git
```

Build custom PHP docker image with necessary extensions
```
cd ~/chesscheat/cc-symfony-docker

docker-compose build
```

Stop local services and other containers which are using the following ports: 80, 443, 8080

Start the container
```
docker-compose up -d
```

Check the services are running *(php, maria, adminer, httpd)*
```
lh@lh-VPCSA35GG:~/chess/cc-symfony-docker$ docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                                                      NAMES
cacfe7f229ea        httpd:2.4.39-alpine    "httpd-foreground"       11 seconds ago      Up 9 seconds        0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp                   ccsymfonydocker_httpd_1
fb79702d39ad        ccsymfonydocker_php    "docker-php-entrypoi…"   12 seconds ago      Up 10 seconds       9000/tcp                                                   ccsymfonydocker_php_1
01a4ffc2c41e        adminer                "entrypoint.sh docke…"   12 seconds ago      Up 9 seconds        0.0.0.0:8080->8080/tcp                                     ccsymfonydocker_adminer_1
42d5ed7a1ffc        mariadb:5.5.64         "docker-entrypoint.s…"   12 seconds ago      Up 11 seconds       3306/tcp                                                   ccsymfonydocker_maria_1
```

Install symfony components with composer
```
docker-compose exec php composer install 
```

Patch vendor bundles to work with SQUID properly
```
docker-compose exec php patch -p0 -i squid.patch
```

Apply DB migrations
```
docker-compose exec php bin/console doctrine:migrations:migrate
```

Verify access to adminer using the following URL: http://chesscheat.local:8080/?server=maria&username=root&db=chesscheat (password:root)

Verify access to local copy of Symfony using the following URL: http://chesscheat.local/
