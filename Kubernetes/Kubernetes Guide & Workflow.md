<font color="#7f7f7f"><em>Monday, 24-03-2025 - 12:06</em></font>
#university #thesis 

**Source:** [Kubernetes Tutorial for Beginners (Full Course in 4 Hours)](https://www.youtube.com/watch?v=X48VuDVv0do)

## Index
- [[#1. What is Kubernetes?]]
- [[#2. Kubernetes Architecture]]
- [[#3. Core Kubernetes Components]]
- [[#4. Minikube & Kubectl (Local Dev)]]
- [[#5. Demo Project MongoDB + Mongo Express]]
- [[#6. Namespaces]]
- [[#7. Ingress]]
- [[#8. Helm – Kubernetes Package Manager]]
- [[#9. Persistent Volumes & Storage]]
- [[#10. Deploying Stateful Applications]]

## 1. What is Kubernetes?

**Kubernetes (K8s)** is a **container orchestration platform** that automates the deployment, scaling, and management of containerized applications. It allows running and managing applications across multiple machines in different environments (cloud, on-premise, hybrid).

Kubernetes is ideal for **microservices architectures**, offering:

- **High availability** – no downtime
- **Scalability** – horizontal pod autoscaling (increase/decrease replicates based on resource usage - CPU)
- **Disaster recovery** – automatic failover and restart
- **Load balancing** – evenly distributes traffic across available pods
- **Modularity** – systems are broken into reusable, loosely coupled components

---
## 2. Kubernetes Architecture

### Master vs. Worker Nodes

A **Kubernetes cluster** consists of:

- **Master nodes** – manage cluster state, scheduling, and health.
- **Worker nodes** – run the applications (pods) and has components.

### Master Node Processes

Each master node runs these control plane processes:

1. **API Server** – Gateway to the cluster. Handles client requests, performs authentication and validation.
2. **Scheduler** – Selects suitable worker nodes for new pods based on available resources.
3. **Controller Manager** – Watches the cluster state, triggers scheduling if a pod dies.
4. **etcd** – Distributed key-value store. Stores **desired state** and **current state** of the cluster. (e.g., available resources, cluster health)

> **NOTE:** etcd does not store any application data.

### Worker Node Processes

Each worker node runs:

- **Container Runtime** – Docker, containerd, etc.
- **Kubelet** – Talks to the master node, deploys pods as instructed.
- **Kube Proxy** – Handles networking and forwards service requests to correct pods.

### Kubernetes Characteristics & Properties
##### Self-Healing Behaviour

When actual cluster state (e.g., number of running pods) diverges from desired state (specified in YAML files), Kubernetes **automatically adjusts** to match the desired state by referencing `etcd`. This makes the cluster **self-managing and self-healing**.

##### Load Balancing

Services in Kubernetes distribute incoming traffic to all matching pods, ensuring **even traffic distribution**. This prevents overloading one pod and enables **zero-downtime rolling updates**.

##### Scalability

Kubernetes allows you to **scale applications up or down** by increasing or decreasing the number of pod replicas. This can be done manually (`kubectl scale`) or automatically using **Horizontal Pod Auto-scaler** based on CPU/memory usage through an YAML file.

##### Modularity

Kubernetes encourages **decoupling**. Each app component (frontend, backend, DB) is deployed separately and communicates via services. This improves **maintainability**, **reusability**, and **team collaboration**.

---
## 3. Core Kubernetes Components

### Pods

- Smallest deployable unit in Kubernetes
- Abstraction over containers (abstracts the runtime and interaction)
- Usually runs **one application/container** per pod
- Each pod gets a **unique IP** per lifecycle (IP changes on restart)

### Services

- Provide a **stable IP address** and DNS name to pods (permanent)
- Decoupled from the lifecycle of pods (pods can be recreated with a different IP, service remains the same)
- Tightly chained with deployments/Pods.

- **Service Types:**
    - **ClusterIP** (default, internal only)
    - **NodePort** (exposes service on a port on each node)
    - **LoadBalancer** (for external access) `e.g. http://124.89.101.2:8080`


>[!warning]  Pods IP vs. Services IP
>Pods have dynamic IP addresses - lost on pod restart
>Services expose permanent endpoints (IPs) that automatically routes to the correct pod(s).
>
>==> Use Services instead of Pods IPs.

**Internal-Service.YAML**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector: 
    app: nginx
  ports:
  - protocol: TCP
	port: 80          # Service Port
    targetPort: 8080  # Redirecting Port - Pod
```

**External-Service.YAML**
```yaml
apiVersion: v1
kind: Service
metadata:
	name: mongo-express-service
spec:
	type: LoadBalancer  # External type of service definition
	selector:
		app: mongo-express
	ports:
	- protocol: TCP
		port: 8081       # Service Port
	    targetPort: 8081 # Redirecting Port - Pod
	    nodePort: 30000  # External port/endpoint connection
```

Pods/Deployments only have to specify the Pod port, however Services need to specify the service port and the target port of the desired Pod.

> Labels and Selectors are used to connect the different components, labels working as the name of the component and the selector tagging that name to the current component.

### Ingress

- Exposes services using **custom domains and HTTP paths**
- Requires **Ingress Controller** (e.g., NGINX) to process rules and route traffic
- Avoids exposing services via raw IP:Port (e.g., `http://my-app.com` instead of `http://124.89.101.2:8080`)

### ConfigMap & Secret

- **ConfigMap**: Externalizes configuration (e.g., DB URLs, ENV variables)
- **Secret**: Same as ConfigMap but for **sensitive data** (base64 encoded)
- Can be used in pods as:
    - **Environment variables**
    - **Mounted files** (property files)

### Volumes

- Kubernetes does **not persist data** across pod restarts
- Volumes provide persistent storage (local or cloud-based)
- Used in **stateful apps** like databases

### Deployment

- Defines **desired state** of application
- Automatically creates and manages replicas
- Allows **scaling**, **rolling updates**, **rollback**, and **self-healing**

Deployments manage replica sets that manage Pods that are an abstraction of containers.

> **NOTE:** Deployments are used to "create/deploy" Pods, we **do not** create Pods by themselves because deployments act as blueprints (YAML files) and an abstraction layer over Pods for automation and simplicity purposes.

##### Deployment.YAML + Service.YAML
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx  # Identify a component
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:  # Configuration for the Pod
    metadata:
      labels:
        app: nginx
    spec:  # Configuration for the containers inside the Pod
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 8080
---
# Service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:   # Selectors match the labels of components to route traffic or manage pods
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```

>[!warning] Kubernetes Configuration Files - Self-healing
>Each configuration file has 3 parts:
>1- Metadata
>2- Specification
>3- Status 
>
>The 3-Status is automatically generated and edited by Kubernetes, it contains the actual/real status of our component (runtime status) and K8s uses this for SELF-HEALING - matching the 2-Specification described in our configuration files with the 3-Status, if they do not match then K8s acts in order to fix it.
>
>Example: Our configuration file states 2 replicas but at one point the runtime only has 1 replica, Kubernetes sees this difference and schedules the creation of another replica Pod.

### StatefulSet

- For stateful applications like **databases**
- Maintains persistent identity and storage per pod
- Ensures **one master**, **read replicas**, consistent synchronization in case of DBs

> **Note**: StatefulSet is more complex. Often, DBs are hosted **outside K8s** to simplify things.

---
## 4. Minikube & Kubectl (Local Dev)

### Minikube

Runs a local Kubernetes cluster in a single node (Master + Worker processes) and its good for low resource usage for testing and learning purposes.

### Kubectl

CLI to interact with Kubernetes clusters and perform operations like CRUD.

### Common Kubectl Commands

```sh
# List components information
$ kubectl get nodes/pods/services/deployments/replicaset

# Creates default/automatic deployment
$ kubectl create deployment my-app --image=nginx (options)

# Apply YAML configs to K8s resource/component
$ kubectl apply -f file.yaml

# Deletes the K8s resource/component
$ kubectl delete -f file.yaml

# Edit component YAML file - Auto-generated
$ kubectl edit deployment file.yaml

# Shows the actual cluster state (current replicas, available pods, events, etc.)
$ kubectl get deployment nginx-deployment -o yaml > nginx-deployment-result.yaml


# Debugging/Logging & Acessing interative terminal 
$ kubectl logs <pod-name>
$ kubectl describe pod <pod-name>

$ kubectl exec -it <pod-name> -- /bin/bash

$ kubectl describe service nginx-service  # Describes the service/component

$ kubectl get pod -o wide
```

---
## 5. Demo Project: MongoDB + Mongo Express

**GOAL:** Deploying a two-applications system (MongoDB and Mongo Express) locally on Kubernetes.

**Required Components:**
- 2 **Deployments** (mongodb, mongo-express)
- 2 **Services**
	- Internal service for MongoDB (safety) - no external requests reach it
	- External service (LoadBalancer) for Mongo Express - exposes an endpoint for communication
- 1 **ConfigMap** with DB URLs/Addresses
- 1 **Secret** with MongoDB credentials

**Requests Flow:**
Browser/Client -> External Mongo Express Service -> Mongo Express Pod -> MongoDB Internal Service -> MongoDB Pod (Authentication + Data retrieval)

### Step-by-Step
##### Step 1: Create a Secret (MongoDB Credentials)

Encode values on the terminal in Base64
```sh
$ echo -n 'username' | base64
$ dXNlcm5hbWU=

$ echo -n 'password' | base64
$ cGFzc3dvcmQ=
```

**mongodb-secret.yaml**
```yaml
apiVersion: apps/v1
kind: Secret
metadata:
	name: mongodb-secret
type: Opaque
data:
	mongo-root-username: dXNlcm5hbWU=
	mongo-root-password: cGFzc3dvcmQ=
```

Apply the Secret
```sh
$ kubectl apply -f mongo-secret.yaml
```

##### Step 2: MongoDB Deployment + Internal Service

Create the MongoDB deployment configuration file accessing the secret to generate the environment variables.

**mongodb.yaml**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: mongodb-deployment
spec:
	replicas: 1
	selector:
		matchLabels:
			app: mondodb
	template:
		metadata:
			labels:
				app: mongodb
		spec:
			containers:
			- name: mongodb
				image: mongo
				ports:
				- containerPort: 27017
				env:
				- name: MONGO_INITDB_ROOT_USERNAME
					valueFrom:
						secretKeyRef:
							name: mongodb-secret
							key: mongo-root-username
				- name: MONGO_INIT_DB_ROOT_PASSWORD
					valueFrom:
						secretKeyRef:
							name: mongodb-secret
							key: mongo-root-password
---
apiVersion: v1
kind: Service
metadata:
	name: mongodb-service
spec:
	selector:
		app: mongodb
	ports:
	- protocol: TCP
		port: 27017
		targetPort: 27017
```

```sh
$ kubectl apply -f mongodb.yaml
```

##### Step 3: Create a ConfigMap for DB Host

This will contain the MongoDB URL for connection purposes.

**mongodb-configmap.yaml**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
	name: mongodb-configmap
data:
	database_url: mongodb-service
```

```sh
$ kubectl apply -f mongodb-configmap.yaml
```

##### Step 4: Mongo Express Deployment + External Service

Create the Mongo Express deployment configuration file accessing the secret to generate the environment variables and the config map for the DB URL.

**mongo-express.yaml**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: mongo-express
spec:
	replicas: 1
	selector:
	    matchLabels:
		    app: mongo-express
	template:
		metadata:
		    labels:
			    app: mongo-express
	spec:
	    containers:
		- name: mongo-express
	    image: mongo-express
        ports:
        - containerPort: 8081
        env:
        - name: ME_CONFIG_MONGODB_SERVER
	        valueFrom:
		        configMapKeyRef:
		            name: mongodb-configmap
		            key: database_url
        - name: ME_CONFIG_MONGODB_ADMINUSERNAME
	        valueFrom:
		        secretKeyRef:
		            name: mongodb-secret
		            key: mongo-root-username
        - name: ME_CONFIG_MONGODB_ADMINPASSWORD
		    valueFrom:
		        secretKeyRef:
		            name: mongodb-secret
		            key: mongo-root-password
---
apiVersion: v1
kind: Service
metadata:
	name: mongo-express-service
spec:
	type: LoadBalancer
	selector:
		app: mongo-express
	ports:
	- protocol: TCP
	    port: 8081
	    targetPort: 8081
	    nodePort: 30000  # external port/endpoint connection
```

```sh
$ kubectl apply -f mongo-express.yaml
```

##### Step 5: Accessing the application

```sh
$ minikube service mongo-express-service
```

This opens the Mongo Express UI in the browser and gives information about the service which is the entrypoint for the application (endpoint/URL).

---
## 6. Namespaces

### What is a Namespace?

A **virtual cluster** within a physical cluster. Lets you isolate resources by team, environment, or purpose. Kind of like a package in Java.

##### Minikube Default Namespaces
- kubernetes-dashboard
- kube-system
- kube-public
- kube-node-lease
- default

##### How to create a Namespace

Terminal:
```sh
# Create a namespace
$ kubectl apply -f mysql-configmap.yaml --namespace=namespace

$ kubectl apply -f app.yaml --namespace=team-a
```

Configuration file:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
	name: mysql-configmap
	namespace: my-namespace  # Namespace creation 
data:
	db_url: mysql-service.database
```

### Why Use Namespaces?

- Avoid collisions (e.g., two `my-app` deployments)
- Better organization in large projects
- Limit resources per team/environment

##### Use-Case 1: Large/Complex Projects
Having many K8s components like Deployments, ReplicaSets, Services, ConfigMaps, etc. In a single cluster gives no bigger picture overview.

To manage that we separate these in different namespaces, for example:
- Database namespace
- Monitoring namespace
- Elastic Stack namespace
- Nginx-Ingress namespace

##### Use-Case 2: Different Teams - Same Application
Having different teams working on the same application may cause problems like overriding of configurations. So it is better to have different namespaces for each team.

##### Use-Case 3: Resource Sharing - Environments
Some components are re-used in different environments. For example, a development environment/namespace may have specific Deployments while a production environment/namespace has others but both of these may use the same Nginx Ingress Controller and Elastic Stack so we do not need to replicate these in each environment.

##### Use-Case 4: Blue/Green Deployments
Sometimes its useful to versioning the production versions while a blue production version has X Deployments and the green has Y Deployments they may still use the same Elastic Stack. (variation of use-case 3)

##### Use-Case 5: Access and Resource Limitation
We might want to restrict different teams from other namespaces (isolated environments) while also managing the resources allocation like CPU, RAM, Storage per namespace.

### Namespaces Limitations
ConfigMaps and Secrets are not available for access in between namespaces (they are namespace specific) however Services are.
Moreover, some components are not able to be created inside namespaces, they're global like volumes.

---
## 7. Ingress

### Ingress vs. Services

- Services expose apps using `IP:PORT`
- Ingress exposes apps using **domains** (e.g., `my-app.com`) or **routes** (e.g., `/analytics`)

### Ingress and Ingress Controller

**Ingress** component handles the rules that are set to manage the forwarding of the requests.

**Ingress Controller** component handles the actual forwarding based on the rules set on the Ingress, this controller is often offered/automatic on cloud providers deployments however in local or on-prem deployments it needs to be activated and configured.

**dashboard-ingress.yaml** - Multiple paths for the same host
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
	name: dashboard-ingress
spec:
	rules:
	- host: dashboard.com
		http:
		    paths:
		    - path: /
		        pathType: Prefix
		        backend:
			        service:
				        name: kubernetes-dashboard
			            port:
				            number: 80
```

**dashboard-ingress.yaml** - Multiple subdomains
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
	name: dashboard-ingress
spec:
	rules:
	- host: analytics.dashboard.com
		http:
		    paths:
		        backend:
			        service:
				        name: kubernetes-dashboard
			            port:
				            number: 80
	- host: shopping.dashboard.com
```

```sh
$ kubectl apply -f dashboard-ingress.yaml
```

After this ingress has been setup you may access your application through: `http://dashboard.com/` instead of `http://192.3.22.1:80`.

##### Activating/Enabling Ingress Controller in Minikube

```sh
$ minikube addons enable ingress
```

### Configuring TLS Certificate (HTTPS)

```yaml
apiVersion: v1
kind: Secret
metadata:
	name: myapp-secret-tls
	namespace: default
data:
	tls.crt: base64 encoded cert
	tls.key: base64 encoded key
type: kubernetes.io/tls
```

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
	name: dashboard-ingress
spec:
	tls:
	- hosts:
		- myapp.com
		secretName: myapp-secret-tls
```

---
## 8. Helm – Kubernetes Package Manager

### What is Helm?

- Helm = **Package manager** for Kubernetes (like apt, brew, npm)
- **Helm Charts** = Pre-packaged YAML configs (bundle) for complex setups (DBs, monitoring, Ingress controllers) that are already well established in the market and we do not need to create their configs from the root.
- Helps manage and version our own deployments efficiently

### Why Use Helm?

Manually deploying apps with many components (deployments, services, secrets, configmaps, etc.) is time-consuming. Helm simplifies this by:

- Packaging all necessary K8s configs into a reusable **chart**
- Supporting **templating** to inject custom values - blueprint for deploying and packing microservices
- Tracking installs as **releases**, enabling rollback and upgrade or different environments (dev/staging/prod)

### Helm Chart Structure

```
mychart/
├── Chart.yaml         # Metadata about the chart (name, version, description)
├── values.yaml        # Default variable values
├── templates/         # Template manifests with placeholders
├── charts/            # Subcharts (dependencies)
├── .helmignore        # Files to exclude during packaging
└── README.md          # Optional usage guide
```

**Chart.yaml**
```yaml
apiVersion: v2
name: my-app
version: 0.1.0
description: A Helm chart for deploying my application
```

**values.yaml**
```yaml
replicaCount: 2
image:
  repository: nginx
  tag: 1.17
  pullPolicy: IfNotPresent
service:
  type: ClusterIP
  port: 80
```

Template example for a Deployment - blueprint
**deployment.yaml**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-deployment
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Chart.Name }}
  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}
    spec:
      containers:
      - name: {{ .Chart.Name }}
        image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
        imagePullPolicy: {{ .Values.image.pullPolicy }}
        ports:
        - containerPort: {{ .Values.service.port }}
```

### Using Helm - Commands

```sh
# Create a new chart
$ helm create mychart

# Install chart with release name - Deploys the YAML files (components) into kubernetes 
$ helm install my-release ./mychart

# Install chart with overridden values
$ helm install my-release ./mychart --values=my-values.yaml

# Upgrade release
$ helm upgrade my-release ./mychart

# Rollback to previous release
$ helm rollback my-release 1

# List releases
$ helm list

# Uninstall a release
$ helm uninstall my-release
```

### Where to Find Charts

- [Artifact Hub](https://artifacthub.io/) – official Helm chart repository
- Use `helm repo add` and `helm search repo` to browse and install charts

### Real-World Example

To install **MongoDB** using Bitnami’s Helm chart:

```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-mongo bitnami/mongodb --set auth.rootPassword=secretpass
```

> Use Helm to deploy the same app to multiple environments (dev/staging/prod) with different configs.

Helm brings standardization, repeatability, and simplicity to managing Kubernetes applications at scale.

---
## 9. Persistent Volumes & Storage

### Why Do We Need It?

- Pods are ephemeral: data is lost on restart
- Persistent storage lives outside pod lifecycle

### Requirements

1. Storage must persist outside pod lifecycle
2. Accessible across nodes - **cluster resource not pod resource**
3. Survive cluster failures

### Components

- **PersistentVolume (PV)**: Cluster-level storage resource (NFS, disk, cloud)
- **PersistentVolumeClaim (PVC)**: Requests a volume
- **StorageClass**: Automates volume provisioning (abstract storage provider)

### Mount Example

```yaml
apiVersion: v1
kind: Pod
metadata:
	name: mypod
spec:
	containers:
	- name: myfrontend
		image: nginx
		volumeMounts:
		- mountPath: "/data"
			name: mypod
	volumes:
		- name: mypod
			persistentVolumeClaim:
				claimName: pvc-name
```

---

## 10. Deploying Stateful Applications

### What is a Stateful App?

- Needs consistent identity and persistent data (e.g., databases)

### Issues with Replication

- You can’t run 3 MySQL pods that read/write independently
- One pod must be **primary (read-write)**, others are **replicas (read-only)**
- Needs syncing between storage volumes, data must be updated when a write operation happens on the master/primary database Pod - worker nodes need to know when to update the database.

### StatefulSet

- Assigns **stable network identity**
- Associates **dedicated persistent volumes** to each replica
- Maintains order and scaling integrity

> Best practice: Run DBs **outside Kubernetes** if you want simpler operations.

