Application development team shared static website content that needed to be hosted using a containerized httpd web server on App Server.

- Container Runtime: Docker (Docker Compose)

- Image Used: httpd:latest

- Container Name: httpd

- Host Port: 3001 

- Container Port: 80

- Volume Mapping: /opt/sysops → /usr/local/apache2/htdocs

# 1: Verify Docker Installation
# 1: Verify Docker Installation
docker --version

Output:

Docker version 26.1.3, build b72abbb

Purpose:

Ensures Docker is installed on App Server 2.

graphql
Copy code

# 2: Verify Docker Compose Availability
docker compose version

Output:

Docker Compose version v2.39.1

Purpose:

Confirms Docker Compose v2 is available to deploy containers using compose files.

graphql
Copy code

# 3: Navigate to Docker Compose Directory
cd /opt/docker
pwd

Output:

/opt/docker

Purpose:

Ensures docker-compose.yml is created at the exact required path.

arduino
Copy code

# 4: Create docker-compose.yml File
vi docker-compose.yml

arduino
Copy code

File Content:
version: "3.8"

services:
web:
image: httpd:latest
container_name: httpd
ports:
- "3001:80"
volumes:
- /opt/sysops:/usr/local/apache2/htdocs

diff
Copy code

Purpose:

- Uses httpd latest image
- Names container as httpd
- Maps host port 3001 to container port 80
- Mounts static content without modifying data
5: Start Container Using Docker Compose
vbnet
Copy code
docker compose up -d

Output:

Container httpd Started

Purpose:

Deploys the httpd container in detached mode using Docker Compose.
6: Verify Container Status
vbnet
Copy code
docker ps

Output:

CONTAINER ID   IMAGE          PORTS                  NAMES
5dec210952b2   httpd:latest   0.0.0.0:3001->80/tcp   httpd

Purpose:

Confirms the httpd container is running with correct port mapping.
7: Verify Volume Mount
vbnet
Copy code
docker inspect httpd | grep -A5 Mounts

Output:

"Source": "/opt/sysops",
"Destination": "/usr/local/apache2/htdocs"

Purpose:

Ensures static website content is mounted correctly inside the container.
8: Verify Website Access
javascript
Copy code
curl http://localhost:3001

Output:

<h1>Index of /</h1>
<ul>
 <li><a href="index1.html">index1.html</a></li>
</ul>

Purpose:

Confirms static content is served successfully via the httpd container.
✅ Final Result
pgsql
Copy code
httpd container created using Docker Compose

Container name set as httpd

Host port 3001 mapped to container port 80

Static content mounted from /opt/sysops

Website accessible successfully

No data modified in mounted directories


