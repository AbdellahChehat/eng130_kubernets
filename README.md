## Kubernetes 

### What is it ? 

Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.


### What is the advantages 

Kubernetes automates operational tasks of container management and includes built-in commands for :
- deploying applications
- rolling out changes to your applications
- scaling your applications up and down to fit changing needs
- monitoring your applications
- more—making it easier to manage applications

### Applying node app 

### K8s Deployment Replicas = 3 

<img width="433" alt="Screenshot 2022-11-29 at 13 37 33" src="https://user-images.githubusercontent.com/115224560/204550426-47d4bd5b-8c70-4992-b948-cfbac086f7a2.png">


### Node deployment diagram 

<img width="677" alt="Screenshot 2022-11-29 at 14 05 55" src="https://user-images.githubusercontent.com/115224560/204550480-818ca9ed-2d62-4bf7-adb3-c133d55021ec.png">


### Useful commands:

-  `kubectl create -f nginx-deploy.yml` to create a deployment 
-  `kubectl create -f nginx-server.yml` to create a server 
-  `kubectl delete deploy (metadata name)`
-  `kubectl get service`
-  `kubectl get deploy/deployment`

### In class Task:

- create nginx-deployment.yml
- using the nginx-image
- create 3 pods replicas
- check status
- kubectl get, create, delete, apply etc.
- kubectl get what-info-do-you-wanna-get
- kubectl get, deploy

Step 1: Made a nginx-deploy.yml and added this code nginx-deploy.yml

```
apiVersion: apps/v1
kind: Deployment 


metadata:
  name: nginx-deployment

spec:
  selector:
    matchLabels:
      app: nginx # look for this label to match with k8 service
  
  # Let's create 3 replica sets of this instances/pods
  replicas: 3 
  
  # template to use it's label for K8 service to launch in the browser
  template:
    metadata:
      labels:
        app: nginx
   
    # Let's define the container spec
    spec:
      containers:
      - name: nginx 
        image: agelemerov/eng130-angel-docker:latest
        ports:
        - containerPort: 80

```

- Run `kubectl create -f nginx-deploy.yml` to run the script
- Run `kubectl get deploymen`t to see the deployments running

Then create file called `nginx-service.yml` 

```
# Select the type of API version and type of service/object
apiVersion: v1
kind: Service

# metadata for name
metadata:
  name: nginx-svc
  namespace: default
# specification to include ports Selector to connect to the deployment
spec: 
  ports: 
  - nodePort: 30001 # range is 30000 - 32768
    port: 80
    targetPort: 80

  # let's define the sselector and label to connect to nginx deployment
  selector: 
    app: nginx # this label connects this service to deployment
  # creating NodePort type of deployment
  type: NodePort # also use LoadBalancer - fro local use cluster IP


```