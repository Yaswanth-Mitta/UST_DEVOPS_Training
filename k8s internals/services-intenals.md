## How two contaniers in a pod communicate?
    - These are kernel capabilites - using namespaces 
        accorindly these two can talk via local host


## how wo containers in diffent pods communicate?
    - network space has soemthing has etho container is connect to container D to cbr 
    - this cbr is central thiing it will commuincate 

## how two containers in two differnt pods across two differnt machines is working?
    - comes to cbr checks no one is there - cbr is also connected some plugin and that plugin will connect tothat pod 
    - that magically happens depends on the pluigin which is not managed by k8s ex weae and all 
    - that magically get to knows is like it has cidr range of each node it is very easy for  the newrok plugin to commuincate 
  
## How cluster ip is talking to the pod?
    - there will be table to mapped things like if request reaches to cbr ther eis something like if it is from this range redriect it to this container like that 
    - kubeproxy connected proxy knows what srevides are woking on lables it will randomly connected 
## IP in IP 
    isntead of nating and denation also 
    wrap from data to 



## scenario 
    if we deploy a service if we curl using ip it will work and if we curl using service name it wont work becuase ther will be code dns which will be deployed in in the name space kube-system 


    if you want to create a pod in particular machine 
    me ca mentin in the json code for that nodenmae code using 
    NOdeName : w1

    how to set for group of nodes to crate any machine 


### Taint
    taint means a stain - if you taint a node new pods wont be created in that node so master nodes has taints 

### Toleration 
    if a pod has tolearation x it can be can go nodes having tolearation x and no toleartion it cant go to the tolearatoin x 

### parameters in taints and toleartions  - applied only in scheduling 

    - No schedule - mean any new pod coming unless it has toleartion and dont touch pods running on it no matter the 
    - PreferNOschedule - if other nodes are there in bad condition like high percentage then only use this node even if this pod has no toleartion ok 
    - noexecute - no new pods with that taint and toleartion and kill the previous pods too running 
  

### Affinity and Anti affinity 