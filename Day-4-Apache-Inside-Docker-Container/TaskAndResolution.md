1.	Install Apache2 inside the running container kkloud

2.	Configure Apache to listen on port 8082 (not bound to any specific IP)

3.	Ensure Apache is running and the container remains running


# Access the App Server and verify the container
```
docker ps


Output:

CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS         PORTS     NAMES
d26f068ae353   ubuntu:18.04   "/bin/bash"   Up ...         Up ...                   kkloud

This confirms the container kkloud is running.
```
# Access the container
```
$ sudo docker exec -it kkloud bash

What this does:

Opens an interactive bash shell inside the container.
```
# Verify OS version inside container
```
root@d26f068ae353:/# cat /etc/os-release

Output excerpt:

NAME="Ubuntu"
VERSION="18.04.6 LTS (Bionic Beaver)"
```
# Update package lists
```
root@d26f068ae353:/# apt update


Output:

Package lists retrieved successfully.
```
# Install Apache2
```
root@d26f068ae353:/# apt install apache2 -y


Output excerpt (success):

Setting up apache2 (2.4.29-1ubuntu4.27) ...

Enabling site 000-default.

Apache installed successfully.
```

# Modify Apache listen port to 8082
```
Update ports.conf

root@d26f068ae353:/# sed -i 's/Listen 80/Listen 8082/' /etc/apache2/ports.conf

Update default VirtualHost

root@d26f068ae353:/# sed -i 's/<VirtualHost \*:80>/<VirtualHost \*:8082>/' /etc/apache2/sites-enabled/000-default.conf


Why this is correct:

•	Listen 8082 means Apache will listen on all interfaces (localhost, 127.0.0.1, container IP).

•	This satisfies: “Do not bind to a specific IP or hostname.”
```

# Start Apache service
```
root@d26f068ae353:/# service apache2 start


Output:

* Starting Apache httpd web server apache2

AH00558: Could not reliably determine the server's fully qualified domain name...

(This warning is normal for Ubuntu containers and does not affect service startup.)

```
# Verify Apache service status
```
root@d26f068ae353:/# service apache2 status


Output:

* apache2 is running

Apache is running successfully.
```
# Test Apache on port 8082
```
root@d26f068ae353:/# curl http://localhost:8082

Output (HTML content):

Apache2 Ubuntu Default Page: It works

This confirms Apache is listening correctly on port 8082
```
# Confirm container IP (optional verification)
```
root@d26f068ae353:/# hostname -i


Output:

172.12.0.2

This confirms that Apache will also respond at:

http://172.12.0.2:8082
```

# Resolution Summary
Step	Description	Status
1	Verified running container	Completed
2	Accessed container	Completed
3	Installed Apache2	Completed
4	Configured Apache to listen on port 8082	Completed
5	Ensured Apache service is running	Completed
6	Verified access via browser/curl	Completed

Final Result:

Apache2 is installed, configured to listen on 8082, running successfully inside the kkloud container, and the container remains operational.
________________________________________

