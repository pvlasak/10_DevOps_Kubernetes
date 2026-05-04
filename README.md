# 10 DevOps Kubernetes
repository to demostrate the container orchestration with Kubernetes

# Deployment of MongoDB and MongoExpress in Kubernetes Cluster
- main branch: directory `demo-deploying-application`

1. `mongo.yaml` 
    - configuration of mongoDB deployment
    - mongo deployment of 1 replica pod is set up
    - container of mongoDB image is running inside the deployment 
    - pod container listens on port 27017
    - authentication data (username and password) are referenced from `mongo-secret.yaml`
    - includes defintion of internal service defined as default *ClusterIP* service and listening on port 27017 and approaching pod container of mongoDB application on the same port 27017.
2. `mongo-secret.yaml`
    - contains key value data of username and password encoded as base64
    *echo -n "username" | base64*
3. `mongo-express.yaml` 
    - deployment of mongo-express docker container listening on port 8081
    - environmental variables to authenticate in mongoDB are referenced by secret component 
    - configMap component defines MongoDB URL 
    - variable containing connection string to access mongoDB using username, password and URL is specified. 
    - service of *Loadbalancer* type is created to allow internal communication with mongo-express container on the port 8081 (targetPort) and external communication comming from browser on port 30000 (nodePort) as well. Service of Loadbalancer type has external public IP address assigned. 
4. `mongo-configmap.yaml` 
    - configMap component defining database URL as service name and port

# Minikube and kubectl commands
*minikube start --driver docker* - start minikube as docker container with docker packaged insided the minikube
- *kubect apply -f <yamlfile>* - start or update configuration inside YAML file
- *kubectl get all* - get all components running in minikube
- *kubectl get deployments* - get all deployments
- *kubectl get replicaset* - get replicaset
- *kubectl get pods* - get all pods 
- *kubectl describe service <service_name>* - give a description of service like a port, targetPort, nodePort, type of service, endpoints can be checked here. Endpoint IP should be matching with the IP address of pod running in a deployment, that the service is communicating with. 
- *kubectl get pod -o wide <pod_name>* - extended info about the pod, including its internal IP address
- *kubectl logs <pod_name>* - logs inside the pod. 
- *minikube service <serviceLoadbalancer>* - assign service an external IP address in minikube.

# Ingress 
- ingress branch

- Ingress component defines the routing rules
- Implementation of Ingress is ingress controller - evaluates routing rules (ingress rules) and manages redirections. Many different 3rd party implementations of ingress controller. 
- configure and start Kubernetes NGINX ingress controller in Minikube: *minikube addons enable ingress*
- `/etc/hosts` file defines mapping between the IP address and hostname 
- 

# Secret and ConfigMap as Volumes

- ConfigMap or Secret must be created in the cluster before the pod starts and referencing them. 
- certificate files, authentication data can be part of secret component
- configuration parameters can be part of Configmap component. 
- configmap and secret can be mounted to a container inside a pod as a volume using attribute `volumeMounts`. Attribute `mountPath`referenced the path inside a container where the file is available inside the container. 