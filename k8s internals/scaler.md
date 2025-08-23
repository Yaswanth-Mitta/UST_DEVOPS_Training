## HPA
- Horizontal Pod Autoscaller 
- it iwll increase more pods
## VPA 
  - Vertical pod Autoscaller 
  - it will reserouces of each pod
  - each pod has two things 
    - soft limit - minimum this must be reserved for this 
    - hard limit - after this reach it will kill the pod

## CA
  - Cluster Autoscaller 
  - Increases number of worker nodes 


## Notes
  ### HPA >> VPA
  - Vertical scaling has limit where horizontal scaling has no limit 
  - Uncertainity of number of pods of running on the worker nodes which cause chaos
  - So HPA is only done instead of VPA

### HPA Detial
## requriemtns in file
    Limit - like cpu uasage , memmory 

    updating mertrics 
    usagee 
    minimum 
    maximum 

    like same deploymetn happens same thing hapens it will be added to etcd and all controller things happens 