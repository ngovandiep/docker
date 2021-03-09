Docker
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

(*) Notes: Requires Microsoft Windows 10 Professional or Enterprise 64-bit, or Windows 10 Home 64-bit with WSL 2.

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

**3. Build docker**

By using, window command line/

1/ Go to docker's directory./
2/ Build image by command./
```bash
docker-compose build
```
																						
3/ Build containner.																				
```bash
docker-compose up
```

**4. Access local site**

http://localhost/


## Useful commands inside container

To update composer packages
```bash
composer install
```

To fix PHP code syntax (please run before committing)
```bash
fixer
```

show docker images
```bash
docker ps
```									
inspect a container											
eg : check container service redis										
```bash
docker inspect redis
```	
Check docker network
```bash
docker network ls										
```

Check docker network
```bash
docker network rm NETWORK [NETWORK...]											
```

Get inside a containner
eg:
    docker exec -it docker_nginx_1 bash										
	docker exec -it docker_phpfpm_1 bash										
 
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
