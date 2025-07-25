# Docker & Kubernetes Notes

---

## Multi‑Stage Builds

**Explanation:**  
Multi-stage builds optimize Docker images by using intermediate stages to build artifacts, and only copying the necessary output into the final image.

<details>
<summary>Show Dockerfile</summary>

```Dockerfile
# Stage 1: Build
FROM node:18 as builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Stage 2: Serve with Nginx
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

</details>

**Commands to run:**

```bash
docker build -t multi-stage-app .
docker run -p 8080:80 multi-stage-app
```

---

## Docker Volumes

**Explanation:**  
Volumes are used to persist data generated and used by Docker containers.

### 1. Named Volume Mount (Docker-managed)

```bash
# Create a named volume
docker volume create mydata

# Run a container with a volume mount
docker run -d --name db -v mydata:/data mongo
```

### 2. Bind Mount (Host-managed)

```bash
# Run a container using a bind mount from host directory
docker run -d --name webapp -v /path/on/host:/usr/share/nginx/html nginx
```

**Commands to inspect volumes:**

```bash
docker volume ls
docker volume inspect mydata
```

---

## Docker Networking

**Explanation:**  
Docker networking allows containers to communicate securely within isolated environments.

```bash
# Create a custom network
docker network create mynet

# Run containers on the custom network
docker run -d --name db --network=mynet mongo
docker run -d --name app --network=mynet myapp
```

**Commands to inspect network:**

```bash
docker network ls
docker inspect mynet
```

---

## Docker Compose

**Explanation:**  
Docker Compose helps define and run multi-container apps using a `docker-compose.yml` file.

<details>
<summary>Show docker-compose.yml</summary>

```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
  db:
    image: mongo
    volumes:
      - dbdata:/data/db
volumes:
  dbdata:
```

</details>

**Commands to run:**

```bash
docker-compose up --build
```

---

## Kubernetes Objects

<details>
<summary>Show Pod YAML</summary>

### Pod

**Explanation:**  
A Pod is the smallest deployable unit in Kubernetes, representing a single instance of a running process.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp
    image: myapp:latest
    ports:
    - containerPort: 3000
```

</details>

<details>
<summary>Show ReplicaSet YAML</summary>

### ReplicaSet

**Explanation:**  
ReplicaSet ensures that a specified number of pod replicas are running at any time.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 3000
```

</details>

<details>
<summary>Show Deployment YAML</summary>

### Deployment

**Explanation:**  
A Deployment provides declarative updates for Pods and ReplicaSets.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 3000
```

</details>

<details>
<summary>Show Service YAML</summary>

### Service

**Explanation:**  
A Service in Kubernetes exposes an application running on a set of Pods.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 3000
```

</details>

**Commands to apply:**

```bash
kubectl apply -f pod.yaml
kubectl apply -f replicaset.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get pods,svc,deploy,rs
```

---

### Rolling Updates in Kubernetes

**Explanation:**  
Kubernetes Deployments support rolling updates by updating pods incrementally without downtime.

```bash
# Update the image of deployment
kubectl set image deployment/myapp-deployment myapp=myapp:v2

# Monitor rollout
kubectl rollout status deployment/myapp-deployment
```

---

## Deploying a Containerized App to Kubernetes

**Steps:**
1. Containerize the app using multi-stage Dockerfile.
2. Push image to a container registry.
3. Define Kubernetes Deployment and Service YAMLs.
4. Apply YAMLs with `kubectl apply`.
5. Expose app via Service and access it.

```bash
# Example end-to-end
kubectl apply -f deployment.yaml
kubectl get services
```

---

You’ve now implemented volumes, networks, Compose, Kubernetes core objects, and rolling updates in a complete deployment flow!
