ShoppingFrontEnd_Docker
===============

The Docker environment for [ShoppingFrontEnd](https://redmine.ig.webike.net/gitbucket/Shopping/ShoppingFrontEnd) project.


## Directory structure
```
├─ docker-compose.yml
│
├─ web
|   ├─ nginx
|   ├─ php-fpm
|   ├─ ...
│   └─ Dockerfile
│
├─ app
│   └─ ... (mount to Laravel)
│
├─ www
│   └─ ... (mount to StaticContent)
│
└─ logs
    └─ ...
```

- Laravel app: https://redmine.ig.webike.net/gitbucket/Shopping/ShoppingFrontEnd
- StaticContent: https://redmine.ig.webike.net/gitbucket/StaticContent/StaticContent

Ask your manager if you don't have access permission.

## Setup manual
**1. Install Docker Toolbox on Windows**:

- Download Docker Toolbox for Windows
  https://docs.docker.com/toolbox/toolbox_install_windows/

- Install **Docker Toolbox**:
  Run "DockerToolbox.exe".
  If network setting dialog displayed, choose public network.
  Docker machine will be created on VirtualBox automatically.

- Restart your computer after installation.

- Run **Docker Quickstart Terminal** to create virtual machine for docker.

**2. Modify editor's End-Of-Line (EOL)**

ShoppingFrontEnd_Docker\web\boot.sh

Maker sure EOL of above file is Unix style `\n` (LF).
```
CR+LF > LF
```
Some git-client may convert the EOL to `\r\n` (CR+LF), that means shell script cannot be run in Linux container.


**3. Modify project path**

ShoppingFrontEnd_Docker\docker-compose.yml
In the `volumes` option, by default `app` and `www` folders were used as application and static content paths in container:

- Application will be mounted to `/home/webike/webapps/frontend/`
- Static content will be mounted to `/home/webike/www/`

```
volumes:
  - "./app/:/home/webike/webapps/frontend"
  - "./www/:/home/webike/www"
```

You may modify project paths in above file with your local paths.

Eg:
```
volumes:
  - "../ShoppingFrontEnd/:/home/webike/webapps/frontend"
  - "../StaticContent/IMG_SERVER/www/:/home/webike/www"
```

**4. Update your configurations**
- ShoppingFrontEnd
You must copy your configuration in `.env` file with your local envionment.  
JP: .env.jp_example > .env  
VN: .env.aws_example > .env

- ShoppingFrontEnd_Docker
You must copy your configuration in `web\nginx\conf.d\default.conf` file with your local envionment.  
JP: NOP  
VN: default.conf_aws > default.conf

**5. Create image and container**

- Run **Docker Quickstart Terminal**

- Change storage driver
```
docker-machine stop default
docker-machine rm default
docker-machine create default \
    --virtualbox-no-vtx-check \
    --virtualbox-cpu-count "2" \
    --virtualbox-memory "2048" \
    --virtualbox-boot2docker-url=https://github.com/boot2docker/boot2docker/releases/download/v18.06.1-ce/boot2docker.iso \
    --engine-storage-driver "aufs"
```
Sometimes storage driver error is occurred using "overlay2", so change to "aufs" driver.

- Modify shared folder settings for docker machine:
  Start "**Oracle VM VirtualBox**".
  Select `default` machine in the left side.
  Right-click menu > Settings > Shared Folders.
  Add new shared folder as below:
```
Folder Path: C:\
Folder Name: c
Read-only: Off
Auto-mount: On
Make permanent: On
```
![Mount](wiki/mount.jpg)

- Change IP address
```
cd /c/workspace/ShoppingFrontEnd_Docker
./static-ip.sh --ip 192.168.99.100 default
docker-machine restart default
```

- Run create command
```
cd /c/workspace/ShoppingFrontEnd_Docker/web
docker-compose build
cd ../
./start-docker.sh
```

**6. Access local site**

http://192.168.99.100/


## Useful commands inside container

To restart webserver
```bash
boot
```

To update composer packages
```bash
composer install
```

To clear Laravel config and route cache
```bash
composer cleanup
```

To fix PHP code syntax (please run before committing)
```bash
fixer
```

To check source quality (using SonarQube)
```bash
sonar
```


## Explain docker-compose.yml
Generally docker-compose.yml is put in project root directory.

**build**

Dockerfile and web server setting were put in the `web`folder. We need to load the `Dockerfile` when building the docker image.
```
build: ./web
```
**ports**

To mount container ports to host ports. Syntax: [Host side]:[Container side]

```
ports:
  - "80:80"
  - "443:443"
  - "6379:6379"
```
By default we are going to mount ports 80 (HTTP), 443 (HTTPS), 6379 (Redis cache).

**volumes**

To mount host's folders into container. Syntax: [Host path]:[Container path]
You can use relative path and mount multiple folders from host into container.

Eg. mount `log` folder from host to `/var/log` in container.

```
volumes:
  - "../log/:/var/log"
```


Other docker commands
=====================
- (Optional) Cleanup build folder (run on host machine):
```bash
docker system prune
```

- (Optional) SSH to virtual machine (run on host machine):
```bash
docker-machine ssh default
```

- (Optional) Change virtual machine IP to 192.168.99.100 when needed (run on host machine):
```bash
./static-ip.sh --ip 192.168.99.100 default
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

- (Optional) Stop/start virtual machine (run on host machine):
```bash
docker-machine stop default
docker-machine start default
```
