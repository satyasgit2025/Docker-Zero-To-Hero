Interview Questions & Answers
Docker & Docker Compose (L1 → L4)
________________________________________
🔹 L1 – Junior Level (Fundamentals)
Q1. What is Docker?
Answer:
Docker is a containerization platform that packages an application and its dependencies into a lightweight, portable unit called a container.
Explanation:
Containers ensure consistency across environments (dev, test, prod) and start faster than virtual machines because they share the host OS kernel.
________________________________________
Q2. What is a Docker container?
Answer:
A Docker container is a running instance of a Docker image.
Explanation:
•	Image → Blueprint
•	Container → Running application
Multiple containers can run from the same image.
________________________________________
Q3. What is Docker Compose?
Answer:
Docker Compose is a tool used to define and run multi-container applications using a YAML file.
Explanation:
Instead of running multiple docker run commands, Docker Compose manages all services in a single file.
________________________________________
Q4. What file is used by Docker Compose?
Answer:
docker-compose.yml
Explanation:
This file defines services, images, ports, volumes, networks, and environment variables.
________________________________________
Q5. What does docker compose up -d do?
Answer:
It starts all services defined in the compose file in detached mode.
Explanation:
•	up → create and start containers
•	-d → run in background
________________________________________
🔹 L2 – Mid Level (Practical Usage)
Q6. What is the difference between docker run and Docker Compose?
Answer:
docker run is used to run single containers, while Docker Compose manages multiple related containers.
Explanation:
Compose is preferred for applications like web + database stacks.
________________________________________
Q7. Why do we map ports in Docker?
Answer:
Port mapping exposes container services to the host or external network.
Explanation:
Example:
8085:80
•	Host port: 8085
•	Container port: 80
________________________________________
Q8. What is a volume in Docker?
Answer:
A volume is a mechanism to persist data outside the container lifecycle.
Explanation:
Without volumes, container data is lost when the container stops or is removed.
________________________________________
Q9. Why are volumes important for databases?
Answer:
Databases require persistent storage to avoid data loss during container restarts.
Explanation:
Mapping /var/lib/mysql ensures MariaDB data remains intact.
________________________________________
Q10. Why do we use environment variables in Docker Compose?
Answer:
Environment variables allow dynamic configuration without hardcoding values.
Explanation:
Used for:
•	Database name
•	Username
•	Passwords
________________________________________
🔹 L3 – Senior Level (Design & Troubleshooting)
Q11. Why use PHP with Apache image instead of plain PHP?
Answer:
The Apache-based PHP image includes both PHP runtime and a web server.
Explanation:
This eliminates the need to configure Apache separately.
________________________________________
Q12. What does depends_on do in Docker Compose?
Answer:
It ensures that dependent services start before the current service.
Explanation:
It controls startup order but does not wait for service readiness.
________________________________________
Q13. Why can’t a non-root user run Docker commands directly?
Answer:
Docker socket (/var/run/docker.sock) is owned by root for security reasons.
Explanation:
Granting Docker access is equivalent to granting root-level privileges.
________________________________________
Q14. Why is HTTPS not working on port 8085 in this setup?
Answer:
The Apache container is configured for HTTP only, not SSL/TLS.
Explanation:
Sending HTTPS traffic to an HTTP service results in SSL errors.
________________________________________
Q15. What happens if the database container restarts?
Answer:
The application continues to work because database data is stored in a volume.
Explanation:
Volumes decouple container lifecycle from data lifecycle.
________________________________________
🔹 L4 – Architect Level (Design, Security, Scalability)
Q16. Why use Docker Compose in pre-production environments?
Answer:
Docker Compose provides a simple, reproducible environment for testing multi-service applications.
Explanation:
It reduces environment drift and accelerates testing cycles.
________________________________________
Q17. How would you secure database credentials in production?
Answer:
Use secrets management instead of plain environment variables.
Explanation:
Options include:
•	Docker secrets
•	Vault
•	Cloud-native secret managers
________________________________________
Q18. How would you scale the web service?
Answer:
Use multiple replicas behind a load balancer.
Explanation:
Docker Compose supports scaling, but Kubernetes is preferred for production-grade scaling.
________________________________________
Q19. Why is Docker Compose not ideal for large production systems?
Answer:
Docker Compose lacks advanced orchestration features like auto-healing, rolling updates, and self-scaling.
Explanation:
These features are natively provided by Kubernetes.
________________________________________
Q20. How would you migrate this setup to Kubernetes?
Answer:
Convert:
•	Web service → Deployment + Service
•	DB → StatefulSet + PersistentVolume
Explanation:
Kubernetes provides better resilience, scalability, and observability.
________________________________________
🔹 One-Line Summary (Interview-Ready)
Docker Compose is ideal for defining and testing multi-container application stacks, while production environments benefit from orchestrators like Kubernetes for scalability, resilience, and security.
________________________________________________________________________________
Real-Time Scenario Interview Questions
Docker, Docker Compose, Linux & Containers (L1 → L4)
________________________________________
🔹 L1 – Junior Level (Operational Scenarios)
Q1. A container is running but the application is not accessible from the browser. What will you check first?
Answer:
Check port mapping using:
docker ps
Explanation:
If the container port is not mapped to the correct host port, the application cannot be accessed externally.
________________________________________
Q2. You ran docker ps and the container is not listed. What does this mean?
Answer:
The container is not running.
Explanation:
Use:
docker ps -a
to check if it exited or failed.
________________________________________
Q3. A Docker command fails with “permission denied”. What is the likely cause?
Answer:
The user is not authorized to access the Docker daemon.
Explanation:
Docker requires root privileges or membership in the docker group.
________________________________________
Q4. You updated a PHP file on the host but changes are not reflected in the browser. What could be wrong?
Answer:
The volume mapping may be incorrect or missing.
Explanation:
Without a volume, container changes won’t reflect host file updates.
________________________________________
🔹 L2 – Mid Level (Troubleshooting Scenarios)
Q5. The database container restarts, but the application still works. Why?
Answer:
Because database data is stored in a persistent volume.
Explanation:
Volumes decouple container lifecycle from data lifecycle.
________________________________________
Q6. A container exits immediately after startup. How do you investigate?
Answer:
Check container logs:
docker logs <container_name>
Explanation:
Most startup failures are due to configuration or missing environment variables.
________________________________________
Q7. A developer hardcoded database credentials inside a Dockerfile. Is this acceptable?
Answer:
No, it is a security risk.
Explanation:
Credentials should be injected via environment variables or secrets.
________________________________________
Q8. You exposed port 3306 of the database container publicly. What is the risk?
Answer:
Direct exposure of the database increases attack surface.
Explanation:
Databases should be accessible only within internal networks.
________________________________________
🔹 L3 – Senior Level (Design & Incident Scenarios)
Q9. Your Docker Compose application works locally but fails on the server. What would you compare?
Answer:
Compare:
•	Docker versions
•	Environment variables
•	Volume paths
•	Network configuration
Explanation:
Environment drift is a common root cause.
________________________________________
Q10. Containers start successfully, but the web app cannot connect to the database. What do you check?
Answer:
Verify:
•	DB service name in the application config
•	Correct credentials
•	Container network connectivity
Explanation:
Compose uses service names as DNS hostnames.
________________________________________
Q11. Why does depends_on not guarantee database readiness?
Answer:
It controls startup order but not service health.
Explanation:
Health checks are required for readiness validation.
________________________________________
Q12. During deployment, disk space fills up quickly. What Docker components would you clean?
Answer:
Remove unused:
docker system prune
Explanation:
Dangling images, stopped containers, and unused volumes consume disk.
________________________________________
🔹 L4 – Architect Level (Scalability & Production Scenarios)
Q13. How would you handle zero-downtime deployment for this application?
Answer:
Use rolling updates behind a load balancer.
Explanation:
New containers start before old ones stop, ensuring availability.
________________________________________
Q14. Why is Docker Compose not recommended for large-scale production?
Answer:
It lacks auto-healing, scaling, and rolling update capabilities.
Explanation:
Kubernetes is better suited for production orchestration.
________________________________________
Q15. How would you secure Docker in an enterprise environment?
Answer:
•	Restrict Docker socket access
•	Use non-root containers
•	Scan images for vulnerabilities
•	Implement RBAC
________________________________________
Q16. How would you design backup and recovery for containerized databases?
Answer:
Use scheduled backups to external storage.
Explanation:
Containers are ephemeral; backups must live outside containers.
________________________________________
Q17. Your application experiences high traffic spikes. How do you scale?
Answer:
Scale horizontally by adding replicas.
Explanation:
Stateless services scale better and faster.
________________________________________
Q18. How would you monitor container health in production?
Answer:
Use centralized logging and monitoring tools.
Explanation:
Tools like Prometheus, Grafana, and ELK provide observability.
________________________________________
🔹 One-Line Architect Summary
In real-world containerized environments, success depends not only on running containers but on secure design, observability, scalability, and failure recovery.
________________________________________________________________________________
Production Incident Case Studies
Docker, Containers, Linux & DevOps (Real-World)
________________________________________
🟠 Case Study 1: Application Suddenly Down After Server Reboot
(L2–L3 level)
Incident
After a routine server reboot, the application was not accessible, even though Docker was installed.
Symptoms
•	curl <server-ip>:8085 → Connection refused
•	docker ps → No containers running
Root Cause
•	Containers were started manually using docker run
•	No restart policy or Docker Compose in place
•	Containers did not auto-start after reboot
Resolution
docker compose up -d
Prevention
•	Use Docker Compose with system-managed startup
•	Configure restart policy:
restart: always
Interview Takeaway
Containers are ephemeral; persistence requires orchestration or restart policies.
________________________________________
🔴 Case Study 2: Database Data Lost After Container Restart
(L3 level – Critical Incident)
Incident
After a MariaDB container restart, the application started but all data was missing.
Symptoms
•	Tables missing
•	Fresh database initialized
Root Cause
•	Database was running without a volume
•	/var/lib/mysql stored inside container filesystem
Resolution
•	Stop container
•	Recreate container with volume mapping:
volumes:
  - /var/lib/mysql:/var/lib/mysql
Prevention
•	Never run stateful services without volumes
•	Mandatory checklist for DB containers
Interview Takeaway
Containers should be stateless; data must live outside containers.
________________________________________
🟠 Case Study 3: Application Works Locally but Fails in Production
(L3 level)
Incident
App worked on developer machine but failed on production server.
Symptoms
•	DB connection timeout
•	No error in container startup
Root Cause
•	Application configured to connect to localhost
•	In Docker Compose, services communicate via service names
Resolution
Updated DB hostname to:
mysql_blog
Prevention
•	Never use localhost inside containers
•	Use service discovery (Docker DNS)
Interview Takeaway
In containers, localhost refers to the container itself, not the host.
________________________________________
🔴 Case Study 4: High CPU Usage on Production Host
(L3–L4 level)
Incident
Production server CPU spiked to 100%, impacting all applications.
Symptoms
•	Slow response times
•	High load average
•	Containers running normally
Root Cause
•	No CPU/memory limits on containers
•	One container consumed all host resources
Resolution
Applied resource limits:
deploy:
  resources:
    limits:
      cpus: "1.0"
      memory: 512M
Prevention
•	Always define resource limits
•	Monitor container metrics
Interview Takeaway
Containers share host resources; limits are mandatory in production.
________________________________________
🟠 Case Study 5: Docker Disk Space Full – Deployment Failed
(L2–L3 level)
Incident
New deployment failed with “no space left on device”.
Symptoms
•	Docker build errors
•	Disk usage 100%
Root Cause
•	Old images and stopped containers not cleaned
•	No Docker housekeeping
Resolution
docker system prune -a
Prevention
•	Scheduled cleanup jobs
•	Image lifecycle management
Interview Takeaway
Docker artifacts accumulate silently and must be managed proactively.
________________________________________
🔴 Case Study 6: Security Breach Due to Exposed Database
(L4 – Architect / Security)
Incident
External scan detected open MySQL port accessible from the internet.
Symptoms
•	Port 3306 publicly reachable
•	Unauthorized login attempts in logs
Root Cause
•	Database port exposed using -p 3306:3306
•	No firewall or network isolation
Resolution
•	Removed port exposure
•	Restricted DB access to internal network only
Prevention
•	Never expose DB ports publicly
•	Use private Docker networks
•	Apply firewall rules
Interview Takeaway
Network exposure is the most common container security failure.
________________________________________
🔴 Case Study 7: Zero-Downtime Deployment Failed
(L4 – Architect)
Incident
During deployment, users experienced downtime.
Symptoms
•	Service unavailable for several minutes
•	Old container stopped before new container started
Root Cause
•	Single container instance
•	No load balancer or rolling deployment strategy
Resolution
•	Introduced multiple replicas
•	Added load balancer
Prevention
•	Blue–Green or Rolling deployments
•	Use orchestration (Kubernetes)
Interview Takeaway
High availability requires redundancy and controlled rollout strategies.
________________________________________
🔴 Case Study 8: Docker Socket Exploited
(L4 – Security / Platform)
Incident
A compromised container gained root access to the host.
Root Cause
•	Docker socket mounted inside container:
/var/run/docker.sock
Resolution
•	Removed socket mount
•	Rotated credentials
•	Rebuilt host
Prevention
•	Never expose Docker socket
•	Use least privilege principle
Interview Takeaway
Docker socket access is equivalent to root access on the host.
________________________________________
🔹 How Interviewers Use These Cases
They want to assess:
•	Root cause analysis
•	Production mindset
•	Security awareness
•	Preventive thinking
________________________________________
One-Line Senior-Level Summary
Most production container incidents are not Docker bugs but design, security, or operational mistakes.
________________________________________
Want More?
I can also provide:
•	Postmortem templates
•	Cloud-specific incidents (AWS / OCI / Azure)
•	Linux + Docker combined failures
•	Kubernetes incident case studies
•	Short “What went wrong?” interview answers
Tell me what level or company you’re preparing for.
CI/CD pipeline questions from this task
Below are CI/CD pipeline interview questions derived directly from your Docker Compose (PHP + MariaDB) task, written as real-world pipeline scenarios and mapped from L1 → L4 (Junior → Architect).
These are exactly the kind of questions asked when interviewers review a hands-on DevOps project.
________________________________________
CI/CD Pipeline Interview Questions
Based on Docker Compose PHP–MariaDB Deployment (L1 → L4)
________________________________________
🔹 L1 – Junior Level (CI/CD Fundamentals)
Q1. What is CI/CD in the context of this Docker Compose task?
Answer:
CI/CD automates building, testing, and deploying the PHP–MariaDB application using pipelines instead of manual commands.
Explanation:
In this task, CI would validate the code and Docker configuration, while CD would deploy the containers on the app server.
________________________________________
Q2. Which files from this task would be part of the CI pipeline?
Answer:
•	docker-compose.yml
•	Application files (index.php)
•	Any configuration files
Explanation:
These files define how the application is built and deployed.
________________________________________
Q3. What is the first CI step before deployment?
Answer:
Source code checkout from the repository.
Explanation:
The pipeline must pull the latest code before any build or deployment steps.
________________________________________
Q4. Why should Docker Compose not be run manually in production pipelines?
Answer:
Manual execution is error-prone and not repeatable.
Explanation:
CI/CD ensures consistent, automated deployments.
________________________________________
🔹 L2 – Mid Level (Pipeline Design & Automation)
Q5. How would you automate this Docker Compose deployment in a CI/CD pipeline?
Answer:
By adding a pipeline stage that runs:
docker compose up -d
Explanation:
This ensures consistent deployment after successful validation.
________________________________________
Q6. What validations would you add before running docker compose up?
Answer:
•	YAML syntax check
•	Docker Compose config validation
•	PHP syntax check
Explanation:
This prevents runtime failures.
________________________________________
Q7. How would you prevent deployment if the Docker Compose file is invalid?
Answer:
Use:
docker compose config
Explanation:
This validates the compose file before deployment.
________________________________________
Q8. How would you store database credentials in CI/CD securely?
Answer:
Use CI/CD secret management.
Explanation:
Credentials should never be hardcoded in repositories.
________________________________________
🔹 L3 – Senior Level (Reliability & Rollbacks)
Q9. How would you implement rollback for this deployment?
Answer:
By redeploying the previous known-good Docker Compose version.
Explanation:
Git acts as the source of truth for rollback.
________________________________________
Q10. How do you ensure zero downtime during deployment?
Answer:
Run deployments with multiple replicas behind a load balancer.
Explanation:
Single-container setups cause downtime.
________________________________________
Q11. What would you monitor after deployment?
Answer:
•	Container health
•	Application response
•	Database availability
Explanation:
Post-deployment verification ensures successful release.
________________________________________
Q12. What is a real CI/CD failure scenario in this task?
Answer:
Pipeline fails due to permission issues accessing Docker daemon.
Explanation:
CI runner must have Docker access.
________________________________________
🔹 L4 – Architect Level (Scalable & Secure Pipelines)
Q13. How would you redesign this pipeline for production scale?
Answer:
Move from Docker Compose to Kubernetes.
Explanation:
Kubernetes provides rolling updates, auto-healing, and scaling.
________________________________________
Q14. How would you separate CI and CD responsibilities?
Answer:
CI builds and tests images; CD deploys pre-approved artifacts.
Explanation:
This improves security and traceability.
________________________________________
Q15. How would you enforce security in the pipeline?
Answer:
•	Image scanning
•	Secrets masking
•	Least privilege execution
Explanation:
Security must be embedded in CI/CD.`
________________________________________
Q16. How would you handle database migrations in CI/CD?
Answer:
Use versioned migration scripts executed before deployment.
Explanation:
Schema changes must be controlled and reversible.
________________________________________
Q17. How would you handle multi-environment deployments?
Answer:
Use environment-specific variables and compose overrides.
Explanation:
Same pipeline, different configs.
________________________________________
🔹 Sample CI/CD Pipeline Flow (Based on Your Task)
Git Commit
   ↓
CI: Code Checkout
   ↓
CI: docker compose config
   ↓
CI: PHP syntax validation
   ↓
CD: docker compose up -d
   ↓
Post-Deploy: curl health check
________________________________________
🔹 One-Line Architect-Level Summary
A CI/CD pipeline for Docker Compose ensures automated, validated, and repeatable deployments while reducing human error and enabling faster releases.


