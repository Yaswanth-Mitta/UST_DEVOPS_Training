## Docker & Kubernetes Notes

---

## Topics Covered

- Multi-stage Docker builds
- Docker Volumes  
    - Bind mount
- Docker Networking  
    - Custom bridge network  
    - Container-to-container communication 
- Kubernetes Objects  
    - Pod  
    - ReplicaSet  
    - Deployment  
    - Service
- Rolling updates in Kubernetes  
    - kubectl set image  
    - Rollout status
- Deploying a containerized app on Kubernetes  
    - Docker build & push  
    - Apply YAML files  
    - Expose via Service

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

### Named Volume Mount (Docker-managed)

```bash
docker volume create mydata
docker run -d --name db -v mydata:/data mongo
```

### Bind Mount (Host-managed)

```bash
docker run -d --name webapp -v /path/on/host:/usr/share/nginx/html nginx
```

**Volume Commands:**

```bash
docker volume ls
docker volume inspect mydata
```

---



## Docker Networking

**Explanation:**  
Docker networking allows containers to communicate securely within isolated environments.

```bash
docker network create mynet
docker run -d --name db --network=mynet mongo
docker run -d --name app --network=mynet myapp
```

**Network Commands:**

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
<summary>Pod</summary>

```yaml
# pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.21
    ports:
    - containerPort: 80
```

**Pod Commands:**

```bash
kubectl apply -f pod.yaml
kubectl get pods
kubectl describe pod my-nginx-pod
kubectl delete pod my-nginx-pod
```

</details>

<details>
<summary>ReplicaSet</summary>

```yaml
# replicaset.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
```

**ReplicaSet Commands:**

```bash
kubectl apply -f replicaset.yaml
kubectl get rs
kubectl describe rs nginx-replicaset
kubectl delete rs nginx-replicaset
```

</details>

<details>
<summary>Deployment</summary>

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
```

**Deployment Commands:**

```bash
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl describe deployment nginx-deployment
kubectl delete deployment nginx-deployment
```

</details>

<details>
<summary>Service</summary>

```yaml
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
```

**Service Commands:**

```bash
kubectl apply -f service.yaml
kubectl get svc
kubectl describe svc myapp-service
kubectl delete svc myapp-service
```

</details>

---

## Rolling Updates in Kubernetes

**Explanation:**  
Kubernetes Deployments support rolling updates by updating pods incrementally without downtime.

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.22
kubectl rollout status deployment/nginx-deployment
kubectl rollout history deployment/nginx-deployment
# Update image
kubectl set image deployment/nginx-deployment nginx=nginx:1.23

# Check rollout status
kubectl rollout status deployment/nginx-deployment

# Rollback if needed
kubectl rollout undo deployment/nginx-deployment

# Check image and pod details
kubectl get pods -l app=nginx
kubectl describe pod <pod-name>

# Create service (if not already)
kubectl apply -f service.yaml

# Get external IP
kubectl get svc nginx-service

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
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get services
```

You’ve now implemented volumes, networks, Compose, Kubernetes core objects, and rolling updates in a complete deployment flow.
