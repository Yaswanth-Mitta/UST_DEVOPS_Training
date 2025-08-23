# 📦 Docker Volumes

In Docker, **volumes** are used to **persist data** generated and used by containers.  
There are mainly three types:

1. **tmpfs (temporary in-memory storage)**
2. **Bind Mounts**
3. **Volumes (Named & Anonymous)**

---

## 1️⃣ tmpfs (Temporary File System)

- Stored **in memory only** (not on disk).  
- Data is **lost when the container stops**.  
- Useful for **sensitive data** (e.g., credentials) or **caching**.

### Syntax:
```bash
docker run --tmpfs /container/path <image>
```

or

```bash
docker run -v type=tmpfs,destination=/container/path <image>
```

👉 Example:
```bash
docker run --rm -d   --name tmpfs-demo   --tmpfs /app/cache   alpine sleep 1000
```
This mounts a **tmpfs** at `/app/cache` inside the container.

---

## 2️⃣ Bind Mounts

- Mounts a **host directory or file** into a container.  
- Useful for **development** (sharing code between host & container).  
- **Disadvantages**:
  - Security issues (container can modify host files directly).  
  - Depends on host file system.  

### Syntax:
```bash
docker run -v /host/path:/container/path <image>
```

👉 Example:
```bash
docker run -it --rm   -v /home/user/data:/app/data   alpine sh
```
This mounts host folder `/home/user/data` into `/app/data` in the container.

---

## 3️⃣ Volumes

Unlike bind mounts, **Docker manages volumes**.  
- Stored under `/var/lib/docker/volumes/<volume-name>/_data/`  
- Safer than bind mounts (not directly exposed to host users).  
- Easier to **share between containers**.  

There are **two types**:

### (a) Anonymous Volumes
- Created automatically when you **don’t provide a name**.  
- Hard to manage since Docker gives them random names.

👉 Example:
```bash
docker run -v /app/data alpine
```
Here, Docker creates an **anonymous volume** mounted to `/app/data`.

---

### (b) Named Volumes
- Explicitly created and given a name.  
- Easier to reuse and manage.  

👉 Create a volume:
```bash
docker volume create my-volume
```

👉 List volumes:
```bash
docker volume ls
```

👉 Remove a volume:
```bash
docker volume rm my-volume
```

👉 Use it in a container:
```bash
docker run -d   -v my-volume:/app/data   alpine sleep 1000
```
This mounts `my-volume` at `/app/data`.

---

## 📌 Quick Reference

| Type          | Where Data Lives                                | Persistent? | Typical Use Case      |
|---------------|-------------------------------------------------|-------------|-----------------------|
| **tmpfs**     | Memory (RAM)                                    | ❌ No       | Caching, secrets      |
| **Bind**      | Host FS                                         | ✅ Yes      | Dev, config files     |
| **Named**     | `/var/lib/docker/volumes`                       | ✅ Yes      | Databases, logs       |
| **Anonymous** | `/var/lib/docker/volumes` (random name)         | ✅ Yes (but hard to track) | Temporary container data |

---

✅ **Rule of Thumb:**  
- Use **Bind Mounts** for **development**.  
- Use **Volumes** for **production & persistence**.  
- Use **tmpfs** for **temporary, sensitive data**.  
