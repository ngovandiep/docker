Docker setup
===============

The Docker environment project.

## Directory structure
```
├─ docker-compose.yml
│
├─ mysql
│   └─ my.cnf
├─ nginx
|   ├─ conf.d
|   ├─ ...
│   └─ Dockerfile
│
├─ php
|   ├─ config
|   ├─ ...
│   └─ Dockerfile
│
├─ .env
│
```

## Setup manual
**1. Install Docker Desktop on Windows**:

- Download Docker Desktop for Windows.
  https://hub.docker.com/editions/community/docker-ce-desktop-windows/

  (*) Notes: Requires Microsoft Windows 10 Professional or Enterprise 64-bit,</br>
  or Windows 10 Home 64-bit with WSL 2.

**2. Modify project path**

In file docker\.env

```
SOURCE_PATH=<SOURCE DIRECTORY>
VOLUME_PATH=/var/www/html1
```

- Application will be mounted to `/var/www/html1`
 
You may modify project paths <SOURCE DIRECTORY> in above directory with your local paths.

Eg:
```
SOURCE_PATH=C:\workspace\laravel-react-project
```

- Modify others configurations if needed.</br>
eg: you can modify the port number of http/https instead of the default configuration shown below.
```
APP_PORT=80
APP_SSL_PORT=443
```

**3. Check configuration of Nginx server**

Directory docker\nginx\conf.d\default.conf
```
server {
    ...
    root  /var/www/html1/public;
    ....
    location ~ \.php {
      fastcgi_pass            docker_phpfpm_1:9000;
    ...
    }
}
```
Explain
- root : the VOLUME_PATH, the project mounted directory
- docker_phpfpm_1 : phpfpm containner, the generated default name

**4. Build docker**

By using, window command line<br />

1/ Go to docker's directory.<br />
2/ Build image by command.<br />
```bash
docker-compose build
```

3/ Build containner.																				
```bash
docker-compose up
```

if it's all successed, all the services are running as image bellow
![Mount](wiki/docker-desktop.JPG)

**5. Access local site**

http://localhost/





## Useful commands inside container

To update composer packages
```bash
composer install
```

Show docker images
```bash
docker ps
```									

Inspect a container											
eg : check container service redis</br>
```bash
docker inspect redis
```

Check docker network
```bash
docker network ls										
```

Remove specific docker network
```bash
docker network rm NETWORK [NETWORK...]											
```

Get inside a containner
eg:
```bash
    docker exec -it docker_nginx_1 bash
    docker exec -it docker_phpfpm_1 bash
```

Other docker commands
=====================
- (Optional) Cleanup build folder (run on host machine):
```bash
docker system prune
```

- (Optional) Show all images (run on host machine):
```bash
docker images
```

- (Optional) Clean all images and containers (run on host machine):
```bash
docker rm $(docker ps -a -q)
docker rmi $(docker images -q)
```
