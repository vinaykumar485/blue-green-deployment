# blue-green-deployment

step 1:


create namespace

    kubectl create namespace < namespace name >

crete deployment.yaml file for both blue and green saperatly

    vi blue-deployment.yaml
    vi green-deployment.yaml

create service.yaml file

    vi service.yaml

here most importent part is "version"

    selector:
        app: blue-green-deploy
        version: blue

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

VERIFICATION:

    Verify Deployments
    
    kubectl get deployment -n < namepsace name >

    kubectl describe deployment < deployment name > -n < namepsace >

kubectl get pods -o wide -n , namepsace >

kubectl get pods --show-labels -n < name space >

kubectl get svc -n < namespace >

kubectl describe svc < service name > -n < namespace >


    Check:

    Selector:
    version=blue

        Then verify:

        Endpoints:
        192.168.10.15
        192.168.21.18

            These should be the Blue Pod IPs.

Verify AWS Load Balancer

    kubectl get svc -n blue-gree

Copy the loadbalcer URL :

    a1b2c3d4.us-east-1.elb.amazonaws.com

Open it in a browser:

        http://a1b2c3d4.us-east-1.elb.amazonaws.com

                        or

        curl http://a1b2c3d4.us-east-1.elb.amazonaws.com

If Blue serves:

            "BLUE VERSION"

Everything is working







How is rollback done in Blue-Green?
-----------------------------------

Both Blue and Green are already running.

"You simply change the Service selector"

Current Service:

selector:
  app: blue-green-deploy
  version: green

Rollback:

selector:
  app: blue-green-deploy
  version: blue

Apply it:

    kubectl apply -f service.yaml

That's it.








