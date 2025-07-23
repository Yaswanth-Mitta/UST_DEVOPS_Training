Kubernetes - orchestrization tool 
initial name - borg developperd by google open source and managed by cncf 

scheduler - to schedule containner or pods
pods - group of ocntainers 
leader - nodes with manages workeres 
workere - noes with does actual work 
controller/ manger - checks everthing whether it works or not


kubelet - worker level manager , checks whether worker nodes works or not

service proxy - as a user you can connect to worker via apis

network - which leader and worker nodes communicates 

kubectl - controller 


!.Master   - Multile woker nodes 
api servver     kubelet 
scheduler        pods
etcd            servie proxy 
        cni - container netwokr interface connects both