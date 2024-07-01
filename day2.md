# Lecture Plan

1. Docker
2. Kubernetes
3. Model Garden
4. Vertex AI Pipelines


---

# Docker


- Docker is a open source platform for developing, shipping and running applications. 
- Docker allows to package and run an application  in a loosely isolated environment called a container.


**Docker Architecture**


![Docker Architecture](./images/docker-architecture.webp)


**Docker Client**: Docker client communicates with docker daemon using docker API.

**Docker daemon** : listens for docker api requests. manages docker objects (images, containers, volumes and netwrorks).

**Docker image**: Template with instructions to create docker conatiner. contains everything thats necessary to run an application( socurce code + config files). Also contains a execution command that is executed when the image in run in a container.

**Docker Container**: An isolated enviorment to run docker images.

**Docker Registry**: A store for docker images. 

- private registry
- public registry: docker hub.


**Docker Commands**

1. `docker ps`: list running containers
2. `docker ps -a` list all containers
3. `docker image ls` : list all images in private registry
4. `docker run [-d](detached mode) -p ` : to run the image in a container. (`docker create image-name` + `docker start container-id`)
5. `docker build path/to/project -t name:version` : to build a docker image
6. `docker pull`: to pull the image from the docker hub.
7. `docker push`: to push the image to  the docker hub.
- create a repository in docker hub
- tag the image `dokcerhubid/name:version`
- `docker push image-name`
1. `docker start container-id`: to start a container
2. `docker stop container-id`: to stop a conatiner. it waits for program to stop running.
3.   `docker kill container-id`: to stop a container
4.   `docker rm container-id` : to delete a container
5.   `docker image rm image-id` : to delete an image
6.   `docker system prune` : all the stopped containers and unused images, networks and volumes are deleted.
7.   `docker exce container -it id command [sh](shell)` : to execute a commands inside of the container.
8.   `docker logs  container-id` : show log files of a container
9.   `docker logs  --since time container-id` : show log files of a container for a specific period of time
10.  `docker logs  --tail n container-id` : show log files of a container with count n.
11.  `docker init` : helpfull to build an image with guided procedure.

-it : -i +-t

`-i` : keeps SDIN open
`-t` : makes sure that the text is entered and output text is formated properly.



VM Isntance


1. `sudo yum install -y docker`
2. `sudo service docker start`
3. `sudo service docker stop`
4. `sudo service docker status`


# Kubernetes

**Kubernetes or K8s**

- Manual deployment of containers is difficult to maintain and error prone.
- Kubernetes is an open source container orchestartion service.
- crash detection, autoscaling, load balancing are few features that are provided by kubernetes


**Architecture**


![kubernetes architecture](./images/kubernetes-cluster-architecture.svg)


**Pods(container):**

- smallest possible unit.
- conatins one or more containers that run applications.
- hosts one or more applications and their resources.
- created and managed by kubernetes


**Nodes:**

- physical or VM with certain hardware capacity. It hosts pods.

**1. Woker Mode:** 

- runs containers(pods)
- kubelet (monitors the pods)
- kube-proxy (incoming and outgoing traffic.)

**2. Master Node (Control plane):** contols and manages worker nodes.

- API Server: API for the kubelets to communicate
- Scheduler: watches pods for new pods. It selects worker nodes to run the pods.
- Kube control manager: watches and controls worker nodes, manages the no of pods, jobs and ServiceAccounts for a namespace
- cloud control manager: KCM for a cloud provider like AWS EKS or Azure  AKS or GCP GKS or kubermatics.


**Cluster:** a set of nodes that run the containerized appplications(worker node) or control other nodes(master node).

1. kubectl : used to execute commands on  kubernetes cluster.

[kubernetes components](https://kubernetes.io/docs/concepts/overview/components/)
[kubernetes architecture](https://kubernetes.io/docs/concepts/architecture/)

**Steps for Intsallation:**

1. install chocolatey [windows ps](https://community.chocolatey.org/courses/installation/installingl)
2. install kubectl

```
choco install kubernetes-cli
```

3. minikube 

alternatives: microK8's and K3's

```
choco install  minikube
```


to start minikube


```bash
minikube start --driver=docker

# or

minikube start --driver=hyperv

# or
minikube start --driver=virtualbox
```

for status:

```bash
minikube status
```

dashboard

```
minikube dashboard
```


**Kubernetes Objects**

1. pod: Kubernetes will start, stop and replece pods as needed. pods are ephemeral

<i><b>Note:</b></i> whenever a container crashes, the container state and all teh files that were created during the lifetime of container are lost. kubectl will restarct the container with a clean state.

2. deployment: used to manage the pods. Internally is a controler object

- set desired target state. which pods and containers whould run and also the no of instances
- can be paused, deleted and rolled back. 

3. volumes: volumes are used as data stores or pods.

- usefull when a shared filesystem is required.


to create a deployment:

```
kubectl create deployment [name] --image=image-name,image2,image3
```

list deployments

```
kubectl get deployments
```

list pods

```
kubectl get pods
```

delete deployment

```
kubectl delete deployments [name]
```

1. instructions are sent to control plane (master node)
2. scheduler analyzes pods that are currently running and finds the best node for new pods.
3. worker node: kubelet monitors the pods.
4. pods : one container with the image.


- cluster ip is internally  routed through the cluster network.
- service groups pods together and gves them a shared ip
- service allows you to access pods externally.
- without the service communication with pods is difficult internally and impossible to achieve externally.

creating a service:
```
kubectl expose deployment [deployment-name] --type=ClusterIp/NodePort/LoadBalancer
```

list the services

```
kubectl get services
```

```
minikube service [name]
```


to scale:

```
kubectl scale deployment/name --replicas=n
```

### Declerative approach

- info can be passed to kubectl in a mainifest file.
- yaml file or json file
- kubectl will convert this yaml file info to json or other supported format.

**apiVersion:** the version of the kubernetes API that is being used to crerate the object.
**kind:** what kind of object you are creating.

```
kubectl apply -f=file.yaml, file2.yaml
kubectl apply -f=file.yaml -f=file2.yaml
```

#### rollouts

create new rollout:

```
kubectl set image deployment/[name] container-name=new-images
```

status

```
kubectl rollout status deployment/[name]
```

undo the deployment

```
kubectl rollout undo deployment/[name] [--to-revision=n]
```

history

```
kubectl  rollout history deployment/[name] [--revision=n]
```

**labels:** labels are used select a set of objects

ex:

```
enviorment: dev
enviorment: test
enviorment: prod

service: user
service: job
service: application
service: auth
```

**label selectors** 

- not unique like names
- many objects have same labels
- using a label selector you can identify a set of objects.


**annoatations**

- metadata for object. not used for object selection

**names:** every object will have a name(unique in namespace) and UID (unique in cluster).

**namespaces:** 
- namespaces are used to provide isolated environment for multiple users in a single cluster.
- namespace will provide scope for object names.
- namespaces cant be nested.


**Initial Namespaces created for a cluster:**

1. **default**: default namespace where all the objects created without explicit namespace name are located.
2. **kube-public**: any resources in public  namespace is accessible throughout the cluster.
3. **kube-system**: The namespace for objects created by kubernetes systems.
4. **kubernetes-node-lease:** holds the lease objects of each node. lease allows to detect if the nodes are alive. 

list namespaces
```
kubectl get namespaces
```

list the objects
```
kubectl get <object> --namespace=name or -n dev
```

create objects
```
kubectl create <object> <name> -n <=namespace>
```

delete namespace

```
kubectl delete namespace <namespace>
```

#### Network policies

- control the traffic at the IP address kevel or port level.
- container to conatiner communications in a pod is done using `localhost`
- external communication is estblished using service
- pod to pod communications are controlled using Networkpolicies 

**Ingress**: incomming requests
**Egress**: uoutgoing responses

**Optional Reading**

**etcd:** it is a key value store.

- a service like kubernetes have heavy work loads like managing the nodes, job controll, deployment, load balanding and health monitoring etc.
- to monitor the state kubernetes needs a data store which is consistent. this data store contains all the information related to cluster, nodes and all the other objects in kubernetes

**Lease:** any distributed system needs leases.

- lease provides a mechanism to lock shared resources and coordinate activity between the resources.
- every node has `kubelet` and the kubelet sends heartbeat to the AI Server in the master node.
- Every node has a lease object in the kubde-node-lease namespace.
- every heart beat is a update request to the lease object.
- a key is attached to lease and every lease has TTL. a lease will expire if the cluster doesnt recieve a heartbeat within the TTL period.

### GKE

modes:

1. standard : feasibility for advanced con
2. autopilot: a fully-provisioned and managed cluster configuration