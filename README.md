# IntroToDevOps

## Learning Outcome 3

### Task 6
**Task**: Write a Containerfile/Dockerfile which takes the official Ubuntu base image, installs `apache2`, and sets the default command to start `apache2`. It should set the environment variable `STUDENT` to your username and surname. Port 80 should be exposed. Save it as a file called `LO3_D`.

**Solution**:
```dockerfile
FROM ubuntu:latest

# Set environment variable
ENV STUDENT=your_username_surname

# Install apache2
RUN apt-get update && \
    apt-get install -y apache2 && \
    apt-get clean

# Expose port 80
EXPOSE 80

# Set default command to start apache2
CMD ["apache2ctl", "-D", "FOREGROUND"]
```

## Learning Outcome 4

### Task 7
**Task**: Deploy WordPress and MySQL containers by using Podman. WordPress container should successfully connect to the MySQL database container. Use images `mysql:5.7` and `wordpress:php8.2` from Docker Hub. The WordPress container should be named `wordpress` and the MySQL container should be named `mysql`. The WordPress container should not connect to the database by using the root user. Set required environment variables as you see fit. Note down the command used to complete this task into a file called `LO4_M`.

**Solution**:
Create a file called `LO4_M.sh` with the following content:
```sh
#!/bin/sh

# Run MySQL container
podman run --name mysql -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -e MYSQL_PASSWORD=wordpress -e MYSQL_ROOT_PASSWORD=rootpw -d mysql:5.7

# Run WordPress container
podman run --name wordpress --link mysql:mysql -e WORDPRESS_DB_HOST=mysql:3306 -e WORDPRESS_DB_USER=wordpress -e WORDPRESS_DB_PASSWORD=wordpress -e WORDPRESS_DB_NAME=wordpress -p 8080:80 -d wordpress:php8.2
```

### Task 8
**Task**: Choose and justify an appropriate methodology for running a simple Python application that requires a database for persistent data storage for the following scenarios:
a) Development and testing on a developer laptop
b) Production deployment with at least 10 instances and persistent storage

**Solution**:
- **a) Development and testing on a developer laptop**: 
  Use Docker Compose.
  
  **Justification**: Docker Compose provides an isolated environment and ensures consistency between development and production environments. It is simple to configure and use, and it supports the required database and Python application setup.

- **b) Production deployment with at least 10 instances and persistent storage**:
  Use Kubernetes (K8s) with a managed database service (like Amazon RDS, Google Cloud SQL, or Azure Database).
  
  **Justification**: Kubernetes provides robust orchestration, scaling, and management of containerized applications. It supports automatic scaling, load balancing, and self-healing. Using a managed database service ensures persistent, reliable storage and reduces the operational overhead of managing the database.

## Learning Outcome 5

### Task 9
**Task**: Troubleshoot why the following command is wrong and why the container does not start successfully/terminates immediately. Explain your findings and correct the command. Note down both your findings and the command.
```sh
podman run --name mysql -e MYSQL_USER=mike -e MYSQL_PASSWORD=mike -d mysql:5.7
```

**Findings**:
- The `MYSQL_USER` and `MYSQL_PASSWORD` environment variables are not sufficient for MySQL initialization. The `MYSQL_ROOT_PASSWORD` is required.
- The container will terminate because the MySQL server cannot start without a root password.

**Corrected Command**:
```sh
podman run --name mysql -e MYSQL_ROOT_PASSWORD=rootpw -e MYSQL_USER=mike -e MYSQL_PASSWORD=mike -d mysql:5.7
```

### Task 10
**Task**: What feature of container images enables you to deploy containers across multiple environments without having to care about accidentally changing the application code?

**Solution**:
The feature is the **immutability** of container images. Once a container image is built, it contains a fixed version of the application code and its dependencies. This ensures that the same image can be deployed across different environments (development, testing, production) without the risk of code changes or environmental discrepancies affecting the application’s behavior.

### Task 11
**Task**: Troubleshoot why the following Dockerfile is not building. Explain your findings and correct the command. Note down both your findings and the corrected Dockerfile.
```dockerfile
FROM ubuntu

LABEL VERSION=0.1
LABEL BLOCK=true
LABEL EMAIL=student@racunarstvo.hr

RUN apt update && \
    echo it works"

WORKDIR /tmp/

CMD ["sleep", "10"]
```

**Findings**:
- There is a syntax error in the `RUN` command: the string `echo it works"` has an unmatched double quote.

**Corrected Dockerfile**:
```dockerfile
FROM ubuntu

LABEL VERSION=0.1
LABEL BLOCK=true
LABEL EMAIL=student@racunarstvo.hr

RUN apt update && \
    echo "it works"

WORKDIR /tmp/

CMD ["sleep", "10"]
```

## Learning Outcome 6

### Task 12
**Task**: Deploy the `httpd` application on OpenShift. There should be 2 pods in the deployment and a service load balancing traffic coming to `httpd` pods. Create a route to expose the service. If you decide to use a template, you need to use template called `custom-httpd` or template which has its description set to `Custom HTTPD`. If you are not using a template do not set resource limits and requests.

**Solution**:
```sh
# Login to OpenShift
oc login <OpenShift-API-URL> --token=<Your-Token>

# Create a new project
oc new-project httpd-project

# Deploy httpd application with 2 replicas
oc new-app --name=httpd --template=custom-httpd -e REPLICAS=2

# Expose the service to create a route
oc expose svc/httpd

# Verify the deployment
oc get pods
oc get svc
oc get routes
```

### Task 13
**Task**: Why would you use Docker Swarm instead of OpenShift for a simple startup company not having a dedicated DevOps employee?

**Solution**:
Docker Swarm is simpler and easier to set up compared to OpenShift. It requires less operational overhead and is easier to manage, making it a suitable choice for a simple startup with limited resources and no dedicated DevOps personnel. Docker Swarm integrates seamlessly with Docker CLI and Docker Compose, which many developers are already familiar with, thus reducing the learning curve. Additionally, Docker Swarm is lightweight and requires fewer resources compared to OpenShift, making it cost-effective for small-scale deployments.
```

Feel free to customize the username, surname, and any other specifics where placeholders are used. This structure should help organize the tasks and solutions clearly.
