Application development team shared static website content that needed to be hosted`` using a containerized httpd web server on App Server.

- Container Runtime: Docker (Docker Compose)

- Image Used: httpd:latest

- Container Name: httpd

- Host Port: 3001 

- Container Port: 80

- Volume Mapping: /opt/sysops → /usr/local/apache2/htdocs

# 1: Verify Docker Installation
```
docker --version

Output:

Docker version 26.1.3, build b72abbb

Purpose:

Ensures Docker is installed on App Server
```
# 2: Verify Docker Compose Availability
```
docker compose version

Output:

Docker Compose version v2.39.1

Purpose:

Confirms Docker Compose v2 is available to deploy containers using compose files.
```

# 3: Navigate to Docker Compose Directory
```
cd /opt/docker
pwd

Output:

/opt/docker

Purpose:

Ensures docker-compose.yml is created at the exact required path.
```

# 4: Create docker-compose.yml File
```
vi docker-compose.yml

version: "3.8"

services:
  web:
    image: httpd:latest
    container_name: httpd
    ports:
      - "3001:80"
    volumes:
      - /opt/sysops:/usr/local/apache2/htdocs


Purpose:

- Uses httpd latest image
- Names container as httpd
- Maps host port 3001 to container port 80
- Mounts static content without modifying data
```
# 5: Start Container Using Docker Compose
```
docker compose up -d

Output:

Container httpd Started

Purpose:

Deploys the httpd container in detached mode using Docker Compose.
```
# 6: Verify Container Status
```
docker ps

Output:

CONTAINER ID   IMAGE          PORTS                  NAMES
5dec210952b2   httpd:latest   0.0.0.0:3001->80/tcp   httpd

Purpose:

Confirms the httpd container is running with correct port mapping.
```
# 7: Verify Volume Mount
```
docker inspect httpd | grep -A5 Mounts

Output:

"Mounts": [
    {
        "Type": "bind",
        "Source": "/opt/sysops",
        "Destination": "/usr/local/apache2/htdocs",
        "Mode": "rw",

Purpose:

Ensures static website content is mounted correctly inside the container.
```
# 8: Verify Website Access
```
curl http://localhost:3001

Output:

<h1>Index of /</h1>
<ul>
 <li><a href="index1.html">index1.html</a></li>
</ul>

Purpose:

Confirms static content is served successfully via the httpd container.
```
# Final Result
```
httpd container created using Docker Compose

Container name set as httpd

Host port 3001 mapped to container port 80

Static content mounted from /opt/sysops host to container /usr/local/apache2/htdocs

Website accessible successfully

No data modified in mounted directories
```
# Explain docker inspect httpd | grep -A5 Mounts
```
Explanation (Line by Line)
docker inspect httpd

Displays detailed JSON metadata about the container named httpd

Includes information such as:

Container state

Network settings

Port mappings

Volume mounts

Environment variables

This command is commonly used to verify runtime configuration of a container.

| (Pipe)

Passes the output of docker inspect httpd to the next command

Allows filtering large JSON output efficiently

grep -A5 Mounts

grep Mounts searches for the Mounts section in the output

-A5 means “print 5 lines After the matched line”

This shows:

Source path on the host

Destination path inside the container

Mount type (bind/volume)

Read/Write mode
```
