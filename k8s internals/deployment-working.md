Deployment working 


deployment 

replicaset 

pod


changing name from replicaset to deployment in file will work 

deployment controller will create replicaset thing in the etcd and then replca set controller cones into pictures 


Deployment 
user does this 

Replicas 
added by deployment and replica sontroller comes here 


Pods
repoicaset controller adds pods here 

Rolling update - add few delte feww then add few delete - zero down time deployment 

For updating the the deploymetn 
a inew entry is created by replicaset  in etcd 

first it will make sure atleast half of the requried new are pods created and added to etcd andh these manythigns are deleted from the previous pds and and changes desired to acutal and it will make sure reach the desired coount to requried count  thesse are event driven 


intially if some of the reqursts may go old ones for while the pds are still not running until every pds is replaced we cant make sure where request is goining but it happends very fastly mean the pod creating and takes place 