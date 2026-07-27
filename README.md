# blue-green-deployment

step 1:


create namespace

    kubectl create namespace < namespace name >

crete deployment.yaml file for both blue and green saperatly

    vi blue-deployment.yaml
    vi green-deployment.yaml

create service.yaml file
 here most importent part is "version"
    
    vi service.yaml


we have to mention it carefully , whether it is blue or green, for that paricluare version, trafic will be diverted 

    apply al the file in that namespace

    kubectl apply -f blue-deployment.yaml
    kubectl apply -f green-deployment.yaml
    kubectl apply -f service.yaml

verify :

    kubectl get all -n < namespave name >

describe the service , 

    kubectl describe svc < service name> -n < namespace name> 


there  you can see the "end point" and the "verion" for which the service is routing the request, 

if you have mentiooned the version as blue in service.yaml file, then the request will be moving to blue pods. 



step 2: 



Now , you just chnage the version as green in service.yaml file  and apply it
    
    kubectl apply -f < service.yaml >

and then describe the service, you will see the chnage in end point and version of the service, 

    kubeectl describe svc  service.yaml -n < namespace name >

absorve the end point and the version now,

which indicates thet , trafice is now routing to green pods.
-------------------------------------------------------------------------------------------------------------------------------
