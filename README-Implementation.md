# Steps to implement the TODO app in Kubernetes with microservices using minikube as a kubernetes provider

Goal: Create the differents deployments with their services to entablish a communication between them. 
      Environment variables will be stored in a config map, so when this info must be modified, it will be easy to access

### Starting a cluster in Minikube
In order to complete this challenge, the provider Minikube is going to be used. 
Let's start a cluster:
```minikube start```

### Creating the variables that the deployments will use
Once the cluster is initialized, access the yaml file called "config-map-todo.yaml" to create the environement variables that are going to be used.
Run this command: ```kubectl apply -f config-map-todo.yaml -n todo-app```

### Creating the microservices for the stats
To create the microservices for the stats, access the yaml file called "stats.yaml" and execute this command:
```kubectl apply -f stats.yaml -n todo-app```

This should be the output:
```deployment.apps/stats-cache created
service/stats-cache created
deployment.apps/stats-queue created
service/stats-queue created
deployment.apps/stats-worker created
deployment.apps/stats-api created
service/stats-api created
```

The creation of the components can be also checked by these command:
```kubectl get pods -n todo-app```
```kubectl get services -n todo-app```


### Creating the db (MongoDB) and backend api deployments and services
Now, it is time to create the bd, the db that the app is going to use is MongoDB version 4.
Access the yaml file "mongo-db.yaml" and run this commando in order to create the db:
```kubectl apply -f mongo-db.yaml -n todo-app```

To create the backend api, use the file "backend-api.yaml", this file is going to use some variables that are set in the "config-map-todo.yaml" file.
Proceed to create the api by executing this command:
```kubectl apply -f backend-api.yaml -n todo-app```
 
### Creating the front end 
Finally, to create the frontend, use the file "front-end.yaml". This is the yaml file that will give the visual functionality to the todo-app.
Proceed to create the frontend by executing this command:
```kubectl apply -f front-end.yaml -n todo-app```

Note: Make sure to specify the namespace that is being used, in this case 'todo-app'. Otherwise, the application will not work as expected.

Now, check that all the pods are up and running ```kubectl get pods -n todo-app```

### Final step: create a tunnel to establish a load balancer and see the application working as expected.
In order to test and make the application useful, it is required to implement a load balancer. 
This can be done by:
```minikube tunnel```

Now, the application is hosted in the port 80 and can be accessed by typing in the browser the address '127.0.0.1'. 


