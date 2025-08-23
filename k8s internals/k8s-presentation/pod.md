
```mermaid
sequenceDiagram
    participant User
    participant KubeCtl
    participant APIServer
    participant etcd
    participant Scheduler
    participant Kubelet
    participant ContainerRuntime
    participant CNIPlugin

    User->>KubeCtl: kubectl apply -f pod.yaml
    KubeCtl->>APIServer: POST /api/v1/namespaces/{namespace}/pods
    Note over APIServer,etcd: 1. API Server validates and authenticates the request.
    APIServer->>etcd: Saves the Pod object (Status: Pending)
    etcd-->>APIServer: Confirms save

    Note over APIServer,Scheduler: 2. Scheduler watches for unscheduled Pods.
    Scheduler->>APIServer: GET /api/v1/pods?fieldSelector=spec.nodeName=
    APIServer-->>Scheduler: Returns the new, pending Pod.
    Note over Scheduler: 3. Scheduler performs Predicates (filtering) and Priorities (scoring) to select a node.
    Scheduler->>APIServer: PATCH /api/v1/pods/{name}/binding (Binds Pod to a node)
    APIServer->>etcd: Updates Pod object with 'spec.nodeName'
    etcd-->>APIServer: Confirms update

    Note over APIServer,Kubelet: 4. Kubelet on the assigned node watches for Pods bound to it.
    Kubelet->>APIServer: GET /api/v1/pods?fieldSelector=spec.nodeName={my-node}
    APIServer-->>Kubelet: Returns the Pod object with its node assignment.
    Note over Kubelet,ContainerRuntime: 5. Kubelet initiates Pod creation via CRI.
    Kubelet->>ContainerRuntime: CreatePodSandboxRequest (Creates network/PID namespaces)
    ContainerRuntime->>CNIPlugin: PodSandbox Network Setup via CNI
    CNIPlugin-->>ContainerRuntime: Returns Pod IP address.
    ContainerRuntime-->>Kubelet: PodSandbox created with IP.
    Note over Kubelet,ContainerRuntime: 6. Kubelet initiates container creation.
    Kubelet->>ContainerRuntime: CreateContainerRequest
    ContainerRuntime->>ContainerRuntime: Pulls image and starts container process.
    ContainerRuntime-->>Kubelet: Container started.
    Note over Kubelet,APIServer: 7. Kubelet reports status back.
    Kubelet->>APIServer: PATCH /api/v1/namespaces/{namespace}/pods/{name}/status
    APIServer->>etcd: Updates Pod status (e.g., Running, IP assigned).
    etcd-->>APIServer: Confirms update.
    