**Dockerfile vs Docker Compose YAML**

---

**Dockerfile:**

* A Dockerfile is used to **customize and build Docker images**.
* It allows you to install required packages, copy source code, and define how your application should run inside a container.
* It is the basis for creating a Docker image that can later be run as a container.

**Sample Dockerfile to run a C++ application:**

```
FROM gcc:latest

WORKDIR /app

COPY main.cpp .

RUN g++ -o app main.cpp

CMD ["./app"]
```

---

**Multi-Stage Docker Build:**

* Multi-stage builds are used to create **clean and optimized Docker images**.
* During the build process, temporary tools like compilers (e.g., `g++`, `git`) are needed. But they are **not required in the final image**.
* Multi-stage builds help remove unnecessary files and packages by copying only the needed binaries into a fresh base image.

**Example:**

```
FROM gcc:latest AS builder
WORKDIR /app
COPY main.cpp .
RUN g++ -o app main.cpp

FROM ubuntu:latest
WORKDIR /app
COPY --from=builder /app/app .
CMD ["./app"]
```

To build:

```
docker build -t cpp-app .
```

To run:

```
docker run cpp-app
```

---

**Docker Compose (YAML):**

* Used to **run multiple containers together** with networking, volumes, and shared configuration.
* Suitable for applications requiring databases, caches, etc.

**Basic Commands:**

```
docker compose -f filename.yaml up -d    # Start containers in background
docker compose -f filename.yaml down     # Stop and remove containers
```

You can observe Docker networks being created when using `up` and automatically removed when using `down`.

---

**Transferring Files to EC2 Instance:**

Use `scp` command to transfer files from local to EC2:

```
scp -i ../Mitta.pem mongodb.yaml ubuntu@3.148.221.51:/home/ubuntu/docker-mongo
```

Ensure you have **write permissions** in the target directory:

```
sudo chmod 777 /home/ubuntu/docker-mongo
```

---

**Docker Volumes & Mounting:**

* Containers have **ephemeral (non-persistent) storage**.
* Use volumes or bind mounts to persist data outside containers.

**Example:**

```
docker run -it -v /home/ubuntu/data:/test/data ubuntu
```

Inside container:

```
cd /test/data
ls
```

This maps your host directory `/home/ubuntu/data` to `/test/data` inside the container.

Always use **absolute paths** for volume mounting.

---

This note covers key Docker concepts including Dockerfile, multi-stage builds, Docker Compose usage, file transfer, permissions, and volumes.
