
The objective was to create a complete two-tier containerized stack (Web + DB) with strict requirements on container names, ports, volumes, and environment variables.

---

## Requirements

* Docker Compose file location: `/opt/devops/docker-compose.yml` (must be named exactly)
* Deploy two services: **web** and **db**
* Use Docker Compose (v2 supported)
* Application must be accessible via `curl <server-ip>:8085`

### Web Service Requirements
* Container name: `php_blog`
* Image: `php` with any Apache tag
* Port mapping: `8085:80`
* Volume mapping: `/var/www/html:/var/www/html`

### DB Service Requirements
* Container name: `mysql_blog`
* Image: `mariadb` (preferably latest)
* Port mapping: `3306:3306`
* Volume mapping: `/var/lib/mysql:/var/lib/mysql`
* Environment variables:
  * `MYSQL_DATABASE=database_blog`
  * Custom DB user (non-root)
  * Complex password

---


### Step 1: Verify Docker and Docker Compose

```bash
docker --version

Docker version 27.1.1

docker compose version

Docker Compose version v2.29.1

This confirmed:

Docker is installed and running

Docker Compose v2 plugin is available

Legacy docker-compose binary is not required
```
### Step 2: Verify Docker Compose Directory

```bash
cd /opt/devops

ls -la

total 8
drwxr-xr-x 2 root root 4096 .
drwxr-xr-x 1 root root 4096 ..

This confirmed:

Directory exists

No compose file present initially
```
### Step 3: Verify Web and DB Volume Paths

```bash
ls -la /var/www/html

ls -la /var/lib

This confirmed:

Web root directory exists

/var/lib/mysql needed to be created with elevated privileges
```

### Resolution

The task was completed by creating the required Docker Compose file, preparing host directories, and deploying the stack using Docker Compose.

### Step 1: Create Docker Compose File

```bash
sudo vi /opt/devops/docker-compose.yml

Docker Compose Configuration

version: "3.8"

services:
  web:
    container_name: php_blog
    image: php:8.2-apache
    ports:
      - "8085:80"
    volumes:
      - /var/www/html:/var/www/html
    depends_on:
      - db

  db:
    container_name: mysql_blog
    image: mariadb:latest
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_blog
      MYSQL_USER: bloguser
      MYSQL_PASSWORD: Bl0g@123#Pass
      MYSQL_ROOT_PASSWORD: R00t@Strong#123
```
### Step 2: Prepare Host Volumes

```bash
sudo mkdir -p /var/lib/mysql


Web directory already existed:

/var/www/html
```
### Step 3: Add Test Application File

```bash
sudo vi /var/www/html/index.php

<html>
    <head>
        <title>Welcome to xFusionCorp Industries!</title>
    </head>
    <body>
        <?php
            echo "Welcome to xFusionCorp Industries!";
        ?>
    </body>
</html>
```
### Step 4: Deploy the Docker Compose Stack

```bash
sudo docker compose up -d

Output
✔ Network devops_default  Created
✔ Container mysql_blog    Started
✔ Container php_blog      Started


Verify Running Containers

sudo docker ps

Output
php_blog     php:8.2-apache     Up   0.0.0.0:8085->80/tcp
mysql_blog   mariadb:latest    Up   0.0.0.0:3306->3306/tcp
```
### Verify Application Access
```
curl http://localhost:8085


or

curl http://172.16.238.10:8085

Output
Welcome!
```
### Final Result
```
Docker Compose stack deployed successfully

PHP web application accessible via port 8085

MariaDB database running with persistent storage

All task requirements satisfied without deviations
```
