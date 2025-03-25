<font color="#7f7f7f"><em>Monday, 10-03-2025 - 14:01</em></font>
#university #thesis 

**Source:** [Docker Tutorial for Beginners (Full Course in 3 Hours)](https://www.youtube.com/watch?v=3c-iBn73dDE)

## Index
- [[#1. What is a Container?]]
- [[#2. Containers vs. Images]]
- [[#3. Docker vs. Virtual Machines]]
- [[#4. Basic Docker Commands]]
- [[#5. Docker Demo Project]]
- [[#6. Docker Compose - Running Multiple Services]]
- [[#7. Containerizing an Application - Dockerfile]]
- [[#8. Pushing Images to a Private Repository]]
- [[#9. Deploying a Containerized Application]]
- [[#10. Data Persistence with Docker Volumes]]

## 1. What is a Container?

A container is a way to **package applications** with all their necessary dependencies and configurations. It is a **portable artifact** that can be easily shared and moved. Containers make development and deployment **more efficient** by eliminating environment inconsistencies.

### Where do containers live?
- **Container Repository**: A place to store and manage container images.
- **Private Repositories**: Secure, organization-specific storage for container images.
- **Public Repository**: Docker Hub (default repository for Docker images).

### How do containers impact application development?
**Before Containers:**
- Developers need to install services (e.g., PostgreSQL 9.3, Redis 5.0) manually.
- Installation varies across OS environments.
- Complex setup with multiple failure points.

**After Containers:**
- Applications run in isolated environments.
- Containers come pre-configured with services and start scripts.
- Install an application with a single command.
- Run multiple versions of an application without conflicts.

### Application Deployment
**Before Containers:**
- Developers generate artifacts (e.g., JAR files) and provide manual setup instructions.
- Operations teams configure servers, leading to dependency conflicts and deployment errors.

**After Containers:**
- Developers and operations teams collaborate to package applications as containers.
- No server configuration required except Docker runtime.
- Simple deployment with a single `docker run` command.

---
## 2. Containers vs. Images

- **Image**: A package containing an application, configuration, and dependencies.
- **Container**: A running instance of an image with an isolated environment.

##### How to pull and run an image from a public repository?
```sh
# Pull and run PostgreSQL 9.6 image from Docker Hub
$ docker run postgres:9.6
```

If the image is not available locally, Docker will pull it from the repository.

---
## 3. Docker vs. Virtual Machines

- **Docker** virtualizes the application layer by using the host OS kernel.
- **Virtual Machines (VMs)** virtualize both the OS and applications.
- Docker is more lightweight but requires a compatible host OS.

> **Note:** Running a Linux-based Docker image on a Windows host requires a Linux kernel, whereas VMs can run any OS.

---
## 4. Basic Docker Commands

### Managing Images and Containers
```sh
# Pull an image
$ docker pull <image>

# Run a container from an image (-d for detached mode - releases the console)
$ docker run <image:version>
$ docker run -d <image>

# Start an existing container
$ docker start <container-id>

# Stop a running container
$ docker stop <container-id>

# List all running containers
$ docker ps

# List all containers (running and stopped)
$ docker ps -a

# List all downloaded images
$ docker images

# Container Logs & Debugging
$ docker logs <container-id>
$ docker logs <container-id> | tail      # See last activity
$ docker logs <container-id> -f          # Stream logs - Console writing  
$ docker attach <container-id>
```

### Binding Host and Container Ports
Containers need **host-to-container port binding** to be accessible.

```sh
# Bind host port 3001 to container port 3000
$ docker run -p 3001:3000 --name <my-app> <image>
```

> Multiple containers can use the same internal port, but each host port must be unique.
> These ports are used to connect to the running container through 'my-app://localhost:3001'.

### Accessing a Running Container's Console
```sh
# Navigate container filesystem
$ docker exec -it <container-id> /bin/bash

# Alternative shell access
$ docker exec -it <container-id> /bin/sh
```

---
## 5. Docker Demo Project

### Overview
- Develop a **Node.js** application using **MongoDB** such as a user profile.
- Use **MongoDB Express UI** for database visualization.
- **Containerize the application** and connect it to MongoDB.
- Simulate **Jenkins CI/CD** pipeline pushing images to a private repository.
- Deploy the containerized application using **Docker Compose**.

### Setting Up MongoDB and Mongo-Express
```sh
# Lists networks
$ docker network ls

# Create an isolated network for communication
$ docker network create mongo-network

# Start MongoDB container
$ docker run -d \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  --name mongodb \
  --net mongo-network \
  mongo

# Start Mongo-Express UI
$ docker run -d \
  -p 8081:8081 \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
  --name mongoexpress \
  --net mongo-network \
  -e ME_CONFIG_MONGODB_SERVER=mongodb \
  mongo-express
```

> For containers to be able to communicate they use Docker's internal isolated network which needs to be "initialized".

> After this being setup we would enter localhost:8081 (Mongo-Express UI) and create a user-account db -> create a collection/table 'users'.

##### Example of an application requesting MongoDB service
```Javascript
var MongoClient = require('mongodb').MongoClient();

app.get('/get-profile', function(req, res) {
	var response = res;

	MongoClient.connect('mongodb://admin:password@localhost:27017', function (err, client){
		if (err){ throw err; }

		var db = client.db('user-account');
		var query = { userid: 1 };
		db.collection('users').findOne(query, function (err, result){
			if (err) { throw err; }
			
			client.close();
			response.send(result);
		});
	});
});
```

---
## 6. Docker Compose - Running Multiple Services

Instead of using multiple `docker run` commands, **Docker Compose** simplifies multi-container management.

### Example `docker-compose.yaml` File
```yaml
version: '3'
services:
  mongodb:
    image: mongo
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password

  mongo-express:
    image: mongo-express
    ports:
      - 8081:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
```

### Running Docker Compose
```sh
# Start all containers
$ docker-compose -f mongo.yaml up

# Stop and remove all containers
$ docker-compose -f mongo.yaml down
```

> **NOTE:** Containers do not persist data by default. Restarting a container **erases stored data** unless volumes are used.

> **NOTE:** Docker compose handles the creation of a common network by itself.

---
## 7. Dockerfile - Containerizing an Application

### Steps to Build a Custom Docker Image

1. Create a **Dockerfile** with the following instructions:
```dockerfile
FROM node:13-alpine

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

RUN mkdir -p /home/app   # RUN Executes on the docker container console
COPY . /home/app         # COPY Exectures on the Host machine

CMD ["node", "/home/app/server.js"]   # Executes on the docker container console
```

2. Build an image from the Dockerfile:
```sh
$ docker build -t my-app:1.0 .  # '.' => <allocation/directory of the dockerfile>
```

3. Run the container:
```sh
$ docker run -d -p 3000:3000 my-app:1.0
```

> What Jenkins does is building this docker image based on the Dockerfile commit to the repository. After building it pushes it to a docker image repository.

##### Updating a Dockerfile/Containerized Application
We need to rebuild the image whenever this happens.

```sh
# Deletes container
$ docker rm <container-id>

# Deletes image
$ docker rmi <container-id>


# Debug to check if any containers are using a specific image
$ docker ps -a | grep my-app
```

### Multi-Stage Builds for Smaller Images

When building and deploying production-ready images, using a single Dockerfile might lead to huge image sizes because of the unnecessary build dependencies, we can use multi-stage builds to manage this.

```dockerfile
# First stage: Build the application
FROM node:18 AS builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Second stage: Create lightweight production image
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
CMD ["node", "./dist/server.js"]
```

### Dockerfile Best Practices - For Production

The Dockerfile example showed above works well but lacks optimization such as: 
- Avoiding unnecessary layers
- Minimizing image size
- Setting a non-root user for security
- Health checks

```Dockerfile
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Install dependencies (only production ones)
COPY package*.json ./
RUN npm install --only=production

# Optinal: Install curl if the base image does not have it
RUN apk add --no-cache curl

# Health check endpoint for container monitoring
HEALTHCHECK --interval=30s --timeout=10s --start-period=10s --retries=3 \
	CMD curl -f http://localhost:3000/ || exit 1

# Copy source files
COPY . .

# Set proper permissions & drop privileges
RUN chown -R node:node /app
USER node

# Default command
CMD ["node", "server.js"]
```

Extra optimization can include:
- **Minimizing layers:** Combine related `RUN` commands with `&&` to reduce image layers.
- **Use `.dockerignore`:** Exclude unnecessary files (e.g. node_modules, logs).
- **Cache Efficiently:** Copy `package*.json` and run `npm install` before `COPY . .` to leverage layer caching.
- **Use Specific Tags/Versions:** Avoid `latest`; use versioned base images (e.g. `node:18-alpine`).

---
## 8. Pushing Images to a Private Repository

Often docker images are stored in a private repository where developers can push and pull from to different environments.

##### Example
1. Create an ECR (Elastic Container Repository) on AWS - 1 ECR stores a single container with multiple image versions of that container.
2. Push it locally - Authenticate through credentials 

##### Steps to Push an Image to AWS ECR
```sh
# Authenticate to AWS ECR
$ $(aws ecr get-login --no-include-email --region eu-central-1)

# Tag the image for ECR - Serves to link with the specific ECR
$ docker tag my-app:1.0 <AWS-ACCOUNT-ID>.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0

# Push the image
$ docker push <AWS-ACCOUNT-ID>.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
```

---
## 9. Deploying a Containerized Application

##### Steps to Deploy 
1. Log in to the server and pull the application image.
2. Update `docker-compose.yaml` to use the new application image.
3. Change the application external container connections from localhost to mongodb.
4. Run `docker-compose up` to deploy the application.

---
## 10. Data Persistence with Docker Volumes

Containers do not have data persistence by default, meaning that **any data created inside the container is lost once it is stopped or removed**. Docker Volumes solve this by creating a separate persistent storage layer.

### What are Docker Volumes?

Special directories stored outside the container filesystem, managed by Docker.
- Persist data across container restarts
- Are isolated from the host machine (unless explicitly mounted)
- Can be shared across multiple containers

### Basic Volume Usage
```sh
# Create a named volume
$ docker volume create my-volume

# Run a container with a volume mounted
docker run -d -v my-volume:/data --name my-container my-app

# List all volumes
$ docker volume ls

# Inspect a volume
$ docker volume inspect my-volume

# Remove unuser volumes (use with caution)
$ docker volume prune
```

> In the example above, all data written by the container to `/data` will be persisted in the volume named `my-volume`, and it will not be lost when the container is restarted or removed.

### Mounting Host Directories

Mounts a local directory from the host system into the container.

```sh
$ docker run -v $(pwd)/data:/data my-app
```

### Best Practices for Volumes

- Use **named volumes** for shared, persistent data
- Use `.dockerignore` to prevent source volume conflicts
- For databases (like MongoDB/Postgres), map volumes to `/data/db` or equivalent paths
- Back up volumes using `docker run --rm -v <volume>:/data busybox tar czf - /d`
