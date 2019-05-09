# Kubernetes exercise 

## Docker

### Build and Push
Build a docker image and push it to the Docker Hub 
```
cd docker
docker build -t echo-server .
tag echo-server olivernadj/echo-server
push olivernadj/echo-server
```

### Run the public image locally
Run te newly created public image locally as a service
```
docker pull olivernadj/echo-server
docker stop echo-server-container
docker rm echo-server-container
docker run --name=echo-server-container -p 8080:8080 -d olivernadj/echo-server
```

### Run the local image for development purpose
```
docker build -t echo-server .
docker run --name echo-server-container -p 8080:8080 --rm -it echo-server
```

## kubectl examples

Useful commands
```
# check contexs
kubectl config get-contexts

# set current context
kubectl config use-context minikube

# remove all pods, deployments and services
kubectl delete --all pods && kubectl delete --all deployments && kubectl delete --all services

```


### 01-fist-app
```
cd examples/01-fist-app

# create a pod in kubernetes
kubectl create -f echo-server.yaml

# get description of the pod
kubectl get pod
kubectl describe pod echoserver-pod

# execute a command on the running pod
kubectl exec echoserver-pod -it -- bash

# possible command line way to expose the pod
kubectl port-forward echoserver-pod 8081:8080
kubectl expose pod echoserver-pod --type=NodePort --name echoserver-service
kubectl get svc
minikube service echoserver-service --url
kubectl describe svc echoserver-service
kubectl delete svc echoserver-service

# expose with config
kubectl create -f echo-server-service.yaml

# remove recently created kubernetes object
kubectl delete -f echo-server.yaml,echo-server-service.yaml
```

### 02-replication
```
cd examples/02-replication

# create replication controller
kubectl create -f echo-server-repl-controller.yaml,echo-server-service.yaml

# get list of replication controller
kubectl get rc

# remove recently created kubernetes object
kubectl delete -f echo-server-repl-controller.yaml,echo-server-service.yaml
```

### 03-deployment
```
cd examples/03-deployment

# create replication controller
kubectl create -f echo-server-deployment.yaml,echo-server-service.yaml

# get list of deployments
kubectl get deploy

# remove recently created kubernetes object
kubectl delete -f echo-server-deployment.yaml,echo-server-service.yaml
```

### 10-secret-server

You can check out the secret-server repo here:
https://github.com/olivernadj/secret-server-task

```
cd examples/10-secret-server

kubectl create -f goapi-deployment.yaml,grafana-deployment.yaml,nginx-deployment.yaml,prometheus-deployment.yaml,\
redis-deployment.yaml,redis-exporter-service.yaml,goapi-service.yaml,grafana-service.yaml,nginx-service.yaml,\
prometheus-service.yaml,redis-exporter-deployment.yaml,redis-service.yaml

kubectl get deploy
    NAME             READY   UP-TO-DATE   AVAILABLE   AGE
    goapi            3/3     3            3           14s
    grafana          1/1     1            1           14s
    nginx            1/1     1            1           14s
    prometheus       1/1     1            1           14s
    redis            1/1     1            1           14s
    redis-exporter   1/1     1            1           14s

kubectl get svc
    NAME             TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
    goapi            ClusterIP      10.111.203.167   <none>        8080/TCP       43s
    grafana          ClusterIP      10.104.213.211   <none>        3000/TCP       43s
    kubernetes       ClusterIP      10.96.0.1        <none>        443/TCP        38m
    nginx            LoadBalancer   10.101.53.13     <pending>     80:30365/TCP   43s
    prometheus       ClusterIP      10.100.100.79    <none>        9090/TCP       43s
    redis            ClusterIP      10.108.52.230    <none>        6379/TCP       43s


minikube service nginx --url

kubectl delete -f goapi-deployment.yaml,grafana-deployment.yaml,nginx-deployment.yaml,prometheus-deployment.yaml,\
redis-deployment.yaml,redis-exporter-service.yaml,goapi-service.yaml,grafana-service.yaml,nginx-service.yaml,\
prometheus-service.yaml,redis-exporter-deployment.yaml,redis-service.yaml

```


