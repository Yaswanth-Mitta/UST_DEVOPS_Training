## Step 1: kubectl → API Server

- **Authentication**: Verify user identity (certs, tokens, OIDC).  
- **Authorization**: Check permissions via RBAC/ABAC.  
- **Admission Control**: Mutating/validating webhooks adjust or validate the object.  
- **Persistence**: Valid object stored in etcd by API server.  

---

## Step 2: ReplicaSet Controller

- **Detection**: Watches API Server for new/updated ReplicaSets.  
- **Reconciliation**: Compares desired replicas vs actual Pods.  
- **Action**: Creates/deletes Pods to match the desired state.  

---

## Step 3: Scheduler

- **Filtering**: Exclude nodes without enough resources or failing constraints.  
- **Scoring**: Rank remaining nodes (spread, affinity, load).  
- **Binding**: Assign chosen node by updating Pod `spec.nodeName`.  

---

## Step 4: Kubelet (on worker node)

- **Watch**: Detects Pods scheduled to its node.  
- **Pod Sandbox**: Sets up namespaces, volumes, and networking (via CNI).  
- **Container Start**: Pulls images, applies resource limits, launches containers.  

---

## Step 5: Container Runtime & CNI

- **Runtime**: Handles image pull, container lifecycle, sandbox creation.  
- **CNI Plugin**: Configures Pod network, assigns Pod IP.  

---

## Step 6: Status Reporting

- **Kubelet → API Server**: Reports Pod phase (Pending, Running, etc.).  
- **API Server → etcd**: Stores updated Pod status.  

---

## Step 7: Continuous Reconciliation

- **ReplicaSet Controller**: Keeps watching Pods with matching labels.  
- **Self-healing**: If a Pod dies, it creates a replacement.  
- **Scaling**: If replicas change, adds/removes Pods.  



## Container

- **Definition**: A lightweight, standalone, executable package that includes application code, runtime, libraries, and dependencies.  
- **Isolation**: Runs inside its own process space with isolated filesystem, CPU, and memory (via Linux namespaces & cgroups).  
- **Portability**: Can run consistently across environments (developer laptop, test, production).  
- **Lifecycle**: Managed by container runtime (e.g., Docker, containerd, CRI-O).  
- **Limitation**: By itself, does not have self-healing, scaling, or orchestration.  
- **Use case**: The actual unit of execution (e.g., `nginx` image running as a process).  

---

## Pod

- **Definition**: The smallest deployable unit in Kubernetes, which encapsulates one or more containers.  
- **Networking**: All containers in a Pod share the same IP address and network namespace.  
- **Storage**: Can share storage volumes across containers in the Pod.  
- **Lifecycle**: Managed as a single unit; if Pod crashes, Kubernetes won’t recreate it unless managed by a controller.  
- **Use case**: Run one or tightly coupled multiple containers (e.g., Nginx + sidecar for logging).  

---

## ReplicaSet

- **Definition**: A Kubernetes controller that ensures a specified number of identical Pods are always running.  
- **Reconciliation**: Continuously monitors and maintains the desired Pod count.  
- **Self-healing**: Recreates Pods if they are deleted, crash, or fail.  
- **Scaling**: Allows horizontal scaling by changing the `replicas` count.  
- **Ownership**: Uses label selectors to manage Pods, with Pods linked back via `ownerReferences`.  
- **Use case**: Maintain multiple copies of the same Pod for reliability and load distribution.  
