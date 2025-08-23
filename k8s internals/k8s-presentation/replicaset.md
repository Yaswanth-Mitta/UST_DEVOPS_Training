```mermaid
sequenceDiagram
    participant User
    participant KubeCtl
    participant APIServer
    participant etcd
    participant ReplicaSetController
    participant Scheduler
    participant Kubelet
    participant ContainerRuntime
    participant CNIPlugin

    %% User submits the ReplicaSet manifest
    User->>KubeCtl: kubectl apply -f replica-set.yaml
    KubeCtl->>APIServer: POST /api/v1/namespaces/{namespace}/replicasets
    Note over APIServer,etcd: 1. API Server validates and saves the ReplicaSet object.
    APIServer->>etcd: Saves ReplicaSet object
    etcd-->>APIServer: Confirms save

    %% ReplicaSet Controller starts its reconciliation loop
    Note over APIServer,ReplicaSetController: 2. ReplicaSet Controller detects the new ReplicaSet.
    ReplicaSetController->>APIServer: GET /api/v1/replicasets?fieldSelector=metadata.name={rs-name}
    APIServer-->>ReplicaSetController: Returns the ReplicaSet object with 'replicas: 3'
    Note over ReplicaSetController,APIServer: 3. Controller sees current Pod count (0) < desired count (3) and creates Pods.
    ReplicaSetController->>APIServer: Creates Pod object 1
    ReplicaSetController->>APIServer: Creates Pod object 2
    ReplicaSetController->>APIServer: Creates Pod object 3
    Note over APIServer,Scheduler: 4. The Pod creation flow begins for each Pod.
    APIServer->>Scheduler: Notifies scheduler of new Pods.
    Scheduler->>APIServer: PATCH /api/v1/pods/{name}/binding (assigns a node to each Pod)

    %% Kubelet takes over for each Pod
    Note over APIServer,Kubelet: 5. Kubelets on the assigned nodes watch for their Pods.
    Kubelet->>APIServer: GET /api/v1/pods?fieldSelector=spec.nodeName={my-node}
    APIServer-->>Kubelet: Returns assigned Pod objects.
    Kubelet->>ContainerRuntime: CreatePodSandboxRequest (for each Pod)
    ContainerRuntime->>CNIPlugin: Network setup
    CNIPlugin-->>ContainerRuntime: Returns Pod IP
    ContainerRuntime-->>Kubelet: Sandbox created
    Kubelet->>ContainerRuntime: CreateContainerRequest (for each Pod)
    ContainerRuntime->>ContainerRuntime: Pulls image and starts container.
    ContainerRuntime-->>Kubelet: Container running
    Note over Kubelet,APIServer: 6. Kubelet reports Pod status back.
    Kubelet->>APIServer: PATCH /api/v1/pods/{name}/status (e.g., Running)
    APIServer->>etcd: Updates Pod status.

    %% Continuous reconciliation
    loop Reconciliation Loop
        ReplicaSetController->>APIServer: Queries for Pods with matching labels.
        APIServer-->>ReplicaSetController: Returns current count (e.g., 3).
        Note over ReplicaSetController: Compares current (3) vs desired (3) count.
    end