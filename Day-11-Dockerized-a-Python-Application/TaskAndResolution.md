
## Task Overview

A Python-based application needed to be containerized and deployed on **App Server**.  
The application dependencies were already provided via a `requirements.txt` file.
Ppplication had to be packaged into a Docker image and run as a container with specific port mappings.

---

## Requirements

* Application directory: `/python_app`
* Dependency file: `/python_app/src/requirements.txt`
* Create a `Dockerfile` under `/python_app`
* Use any Python base image
* Install dependencies using `requirements.txt`
* Expose container port `8087`
* Run `server.py` using `CMD`
* Image name: `natural/python-app`
* Container name: `pythonapp_natural`
* Port mapping: `8099 (host) → 8087 (container)`
* Validate application using `curl` on App Server 

---

## Initial State
```
The application source code and dependency file were present under:

/python_app/src
├── requirements.txt
└── server.py


Step 1: Verify Application Files
cd /python_app/src
ls -la

Output
requirements.txt
server.py

Step 2: Application Content Verification

cat server.py
======================
from flask import Flask

# the all-important app variable:
app = Flask(__name__)

@app.route("/")
def hello():
    return "Welcome!"

if __name__ == "__main__":
        app.config['TEMPLATES_AUTO_RELOAD'] = True
        app.run(host='0.0.0.0', debug=True, port=8087)
=======================

Key points (for understanding / interview use):

Uses Flask as the web framework

Listens on 0.0.0.0 (required for container access)

Runs on port 8087 (as required by the task)

Returns the message: Welcome!

```

## ==============================Resolution=================================

Step 1: Create Dockerfile
```
The Dockerfile was created under /python_app as required.

sudo vi /python_app/Dockerfile

===============
FROM python:3.9-slim

WORKDIR /app

COPY src/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY src/ .

EXPOSE 8087

CMD ["python", "server.py"]
=================

Explanation (for clarity)

FROM python:3.9-slim → Lightweight Python base image

WORKDIR /app → Sets working directory inside container

COPY requirements.txt → Copies dependency file

RUN pip install → Installs dependencies

COPY src/ . → Copies application code

EXPOSE 8087 → Exposes application port

CMD → Runs the Python app
```
Step 2: Docker Image Build
```
sudo docker build -t nautilus/python-app .
==============
[+] Building 194.9s (10/10) FINISHED                                                                                                                        docker:default
 => [internal] load build definition from Dockerfile                                                                                                                  0.0s
 => => transferring dockerfile: 211B                                                                                                                                  0.0s
 => [internal] load metadata for docker.io/library/python:3.9-slim                                                                                                  124.3s
 => [internal] load .dockerignore                                                                                                                                     0.0s
 => => transferring context: 2B                                                                                                                                       0.0s
 => [1/5] FROM docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338d3060f261f53f144965f755599aab1acda1e13cf1731b1b                                             61.1s
 => => resolve docker.io/library/python:3.9-slim@sha256:2d97f6910b16bd338d3060f261f53f144965f755599aab1acda1e13cf1731b1b                                              0.0s
 => => sha256:b3ec39b36ae8c03a3e09854de4ec4aa08381dfed84a9daa075048c2e3df3881d 1.29MB / 1.29MB                                                                       30.9s
 => => sha256:fc74430849022d13b0d44b8969a953f842f59c6e9d1a0c2c83d710affa286c08 13.88MB / 13.88MB                                                                     30.4s
 => => sha256:2d97f6910b16bd338d3060f261f53f144965f755599aab1acda1e13cf1731b1b 10.36kB / 10.36kB                                                                      0.0s
 => => sha256:dad5b29e3506c35e0fd222736f4d4ef25d21b219acdd73f7bb41d59996ca8e0d 1.74kB / 1.74kB                                                                        0.0s
 => => sha256:085da638e1b8a449514c3fda83ff50a3bffae4418b050cfacd87e5722071f497 5.40kB / 5.40kB                                                                        0.0s
 => => sha256:38513bd7256313495cdd83b3b0915a633cfa475dc2a07072ab2c8d191020ca5d 29.78MB / 29.78MB                                                                     30.8s
 => => sha256:ea56f685404adf81680322f152d2cfec62115b30dda481c2c450078315beb508 251B / 251B                                                                           60.5s
 => => extracting sha256:38513bd7256313495cdd83b3b0915a633cfa475dc2a07072ab2c8d191020ca5d                                                                             2.5s
 => => extracting sha256:b3ec39b36ae8c03a3e09854de4ec4aa08381dfed84a9daa075048c2e3df3881d                                                                             0.6s
 => => extracting sha256:fc74430849022d13b0d44b8969a953f842f59c6e9d1a0c2c83d710affa286c08                                                                             1.5s
 => => extracting sha256:ea56f685404adf81680322f152d2cfec62115b30dda481c2c450078315beb508                                                                             0.6s
 => [internal] load build context                                                                                                                                     0.0s
 => => transferring context: 401B                                                                                                                                     0.0s
 => [2/5] WORKDIR /app                                                                                                                                                0.6s
 => [3/5] COPY src/requirements.txt .                                                                                                                                 0.5s
 => [4/5] RUN pip install --no-cache-dir -r requirements.txt                                                                                                          5.7s
 => [5/5] COPY src/ .                                                                                                                                                 1.1s
 => exporting to image                                                                                                                                                1.4s
 => => exporting layers                                                                                                                                               1.4s
 => => writing image sha256:1892ea775b223689962198d4bb955b48bafa92806dff4ba6b2bbd40aa97ef899                                                                          0.0s
 => => naming to docker.io/natural/python-app                                                                                                                        0.0s
=============

Final confirmation from this output

✔ Base image: python:3.9-slim

✔ Dependencies installed from requirements.txt

✔ Application files copied successfully

✔ Image created and tagged as natural/python-app

✔ Build completed successfully with no error


Verification:

docker images | grep natural/python-app
=======
natural/python-app latest 1892ea775b22 2 minutes ago 133MB
=======
```
Step 3: Container Deployment
```
docker run -d --name pythonapp_natural -p 8099:8087 natural/python-app

==============
64ba0111361df15763dcbca035e1e1b4bdf7e4ce840bb582842e5fa6fa015c88
==============

docker ps
==========
CONTAINER ID   IMAGE                 COMMAND              CREATED          STATUS          PORTS                    NAMES
64ba0111361d   nautral/python-app   "python server.py"   27 seconds ago   Up 24 seconds   0.0.0.0:8099->8087/tcp   pythonapp_ntural
=======
What this confirms (for task/audit use):
Container name: pythonapp_natural

Image used: natural/python-app

Application command: python server.py

Status: Running (Up)

Port mapping: Host 8099 → Container 8087
```

Step 4:Application Validation
```
curl http://localhost:8099

============
Welcome!
===========
```
## Final Requirement Checklist
```
Requirement	Status
Dockerfile under /python_app	✅
Python base image	✅
Install dependencies via requirements.txt	✅
Expose port 8087	✅
CMD runs server.py	✅
Image name natural/python-app	✅
Container name pythonapp_natural	✅
Port mapping 8099:8087	✅
curl validation successful	✅

One-Line Closure Statement (Use for Task / Audit)

Dockerized a Python Flask application using a custom Dockerfile, built the image, deployed it on App Server  with proper port mapping, and validated application accessibility via curl.
```
