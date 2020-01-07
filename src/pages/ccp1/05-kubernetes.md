---
path: "/ccp1/kubernetes"
title: "CCP1 Kubernetes"
date: "2020-01-06"
draft: false
author: Simon Anliker
tags:
  - ccp1
  - cloud
  - kubernetes
---

<!-- KUBE -->

# Containers

Containers vs Virtual Machines:

Hardware -> Host OS -> Hypervisor -> VM1..n (Guest OS -> Libs -> App)

Hardware -> Host OS -> Container Engine -> Container1..n (Libs -> App)


# Kubernetes

Platform for automating deployment, scaling and management of containerized applications. Sits on top of machines (physical or virtual) and provides abstraction of a single machine.


Components in master node (control plane)
- etcd: Key/value store with config and state of cluster
- API Server: Serves the K8 API with JSON
- Scheduler: Selects where to run unscheduled pods
- Controller Manager: Process with core controllers e.g. Replication Controller, DaemonSet Controller

Components on worker nodes (k8 nodes, minions)
- Kubelet: Responsible for running state of the node
- Kube-proxy: Network proxy and load balancer, routes traffic to containers based on IP and port of incoming request
cAdvisor: Monitoring for resource usage and performance metrics of containers
Overlay network: Connects containers on different nodes on a flat network (pluggable e.g.  Weave)


## Main concepts

Cluster: Set of machines where pods are deployed, managed, scaled
- Nodes are connected in a flat network
- Typical cluster size is 1-200 nodes

Controller: Reconciliation loop that drives actual to desired cluster state
- Replication Controller handles replication and scaling
- Replication Controller are being replaced by Deployments

Label: Key-value pairs on any API object
- Label selector to query for objects

Consistent object API with three basic fields
- Object Metadata: Name, UID, version, labels
- Spec: Describes desired state
- Status Read-only information about current state


## Cluster Management

Goals
- Create and connect containers
- Deploy a new version without service interruption
- Take a host down for maintenance without service interruption
- Container failure management for healthy services
- Use as few hosts as possible
- Increase cluster utilization but still meet the application constraints.


Affinity: Containers need to be placed on same node (tight cooperation)

Anti-affinity: Containers need to be placed on different nodes (redundancy)


## Pod

Pod: One or more containers that are guaranteed to be co-located on the same machine
- Shares storage volumes
- Shares networking stack
- Basic unit of scheduling
- Atomic (either up or fail) unit of deployment in Kubernetes
- Containers within the pod can use localhost
- Logical host
- Scheduled on cluster nodes for execution
- Ephemeral
- Instance and restart policy through ReplicaSets
- Cattle, if one dies another one is brought to life

Tightly-coupled containers: multiple containers per pod
Loosely-coupled containers: one container per pod

Pod definition is declaration of desired state.

### Volumes

Shared between containers in same pod with same lifecycle as pod (unless persistent volume)

Types:

emptyDir (erased at pod deletion)

    volumes:
        name: www-data
        emtpyDir: {}
        

hostPath (persisted with host machine lifetime, cloud vendor volumes), e.g. gcePersistentDisk, awsElasticBlockStore, azureFileVolume

    volumes:
        name test-volume
        awsElasticBlockStorage:
            volumeID: <id>
            fsType: ext4
            

secret (used to mount sensitive information, backed by tmpfs which is in RAM)

nfs (persisted network file system)


#### Persistent Volumes

Persistent volumes can be mounted directly or through PersistentVolumeClaim (PVC)

    volumes:
        - name: mypd
          persistentVolumeClaim:
            claimName: myClaim
            

### Usage

Creation

    kubectl create -f pod-nginx.yml
    
List

    kubectl get pods
    
Delete by name

    kubectl delete pod nginx
    
    
    
## Deployments

Replacement for Replication Controller with additional features to update software without disruption.
- Rolling updates
- Rollbacks

Deployments
- Use Replica Sets which implement control loops.
- Work according to the principle of Desired State Configuration
- Changes actual towards desired state at controlled rate


### Usage

View deployments

    kubectl get deployments
    
Check rollout status

    kubectl rollout status deployment/mginx-deployment
    
Update with set

    kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1
    
Update with edit

    kubectl edit deployment/nginx-deployment
    
    
## Replication Set

Rescheduling: on node failure or pod termination

Scaling: change replicas from user command or autoscaling

Rolling updates:
- Create a new replication controller with 1 replica
- Resize the new (+1) and old (-1) controllers one by one
- Delete the old controller after it reaches 0 replicas


### Usage

Creation

    kubectl create -f replication.yml
    
List 

    kubectl get rs 
    
Describe

    kubectl describe rs/apache.zurmo
    
Delete 

    kubectl delete rs/apache.zurmo
 
Resize

    kubectl scale --replicas=COUNT rs/NAME
    
    
    
## Service

Logical set of pods that work together
- Service discovery
- Load balancing
- Routing to IP address and DNS name
    

Services can be exposed in different ways
- ClusterIP: Default. Exposes the service on an internal IP in the cluster, not reachable from outside the cluster
- NodePort: Superset of ClusterIP. Exposes the service on the same port of each selected node using NAT. Accessible from outside the cluster.
- LoadBalancer: Superset of NodePort. Exposes via external load balancer in the current cloud (if supported) and assigns a fixed, external IP
- ExternalName: Kube-dns 1.7 or higher. Exposes using arbitrary name by returning a CNAME record. No proxy used

Pods always receive new IP addresses. Services provide reliable networking endpoints.

Labels are used to assign pods to services. Services define a label selector which is a set of conditions on the label keys.

    apiVersion: v1
    kind: Service
    metadata:
        name: zurmo-apache
    spec:
        ports:
            - port: 80
              protocol: TCP
              targetPort: 80
        selector: 
            name: apache.zurmo
        type: LoadBalancer
        
        
## Multi-host networking

In a cluster we want containers to move freely without port forwarding but still talk to each other without NAT.

Use overlay network (SDN) that provide a flat network. Use packet encapsulation.


## HA deployment

For high-availability the master node can be replicated to avoid failure of the master node.

Three-way replication
- etcd replicates data on each node
- Three load balanced instances of stateless API server
- One elected leader of Scheduler
- One elected leader of Controller Manager
- kubelet as monitor to restart any failing process
- monit monitors and restarts kubelet
    
