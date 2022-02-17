setting up cluster:

Starting k8s locally using minkube

- Ensure docker is running then run the below command
- minikube start --memory=4096 --vm-driver=virtualbox
- To troubleshoot run: minikube status

To get everything in kuberenetes cluster:

- kubectl get all

To see/start k8s dashboard:

- mininkube dashboard

To delete a resource

- kubectl delete <resource e.g po, svc, rs> <name of resource>
- To delete all resources by resource type - kubectl delete <resource e.g po, svc, rs> --all

To get all namespaces

- kubectl get namespaces / kubectl get ns

To get specific resources in a certain namespace

- kubectl get <resource e.g pod, service> -n <namespace>

- View a node resources

kubectl describe node <nodename e.g minikube>

**_ClusterIP vs Nodeport vs Loadbalancer_**

ClusterIP: Services are reachable by pods/services in the Cluster
If I make a service called myservice in the default namespace of type: ClusterIP then the following predictable static DNS address for the service will be created:

myservice.default.svc.cluster.local (or just myservice.default, or by pods in the default namespace just "myservice" will work)

And that DNS name can only be resolved by pods and services inside the cluster.

NodePort: Services are reachable by clients on the same LAN/clients who can ping the K8s Host Nodes (and pods/services in the cluster) (Note for security your k8s host nodes should be on a private subnet, thus clients on the internet won't be able to reach this service)
If I make a service called mynodeportservice in the mynamespace namespace of type: NodePort on a 3 Node Kubernetes Cluster. Then a Service of type: ClusterIP will be created and it'll be reachable by clients inside the cluster at the following predictable static DNS address:

mynodeportservice.mynamespace.svc.cluster.local (or just mynodeportservice.mynamespace)

For each port that mynodeportservice listens on a nodeport in the range of 30000 - 32767 will be randomly chosen. So that External clients that are outside the cluster can hit that ClusterIP service that exists inside the cluster. Lets say that our 3 K8s host nodes have IPs 10.10.10.1, 10.10.10.2, 10.10.10.3, the Kubernetes service is listening on port 80, and the Nodeport picked at random was 31852.

A client that exists outside of the cluster could visit 10.10.10.1:31852, 10.10.10.2:31852, or 10.10.10.3:31852 (as NodePort is listened for by every Kubernetes Host Node) Kubeproxy will forward the request to mynodeportservice's port 80.

LoadBalancer: Services are reachable by everyone connected to the internet\* (Common architecture is L4 LB is publicly accessible on the internet by putting it in a DMZ or giving it both a private and public IP and k8s host nodes are on a private subnet)
(Note: This is the only service type that doesn't work in 100% of Kubernetes implementations, like bare metal Kubernetes, it works when Kubernetes has cloud provider integrations.)

If you make mylbservice, then a L4 LB VM will be spawned (a cluster IP service, and a NodePort Service will be implicitly spawned as well). This time our NodePort is 30222. the idea is that the L4 LB will have a public IP of 1.2.3.4 and it will load balance and forward traffic to the 3 K8s host nodes that have private IP addresses. (10.10.10.1:30222, 10.10.10.2:30222, 10.10.10.3:30222) and then Kube Proxy will forward it to the service of type ClusterIP that exists inside the cluster.

You also asked: Does the NodePort service type still use the ClusterIP? Yes*
Or is the NodeIP actually the IP found when you run kubectl get nodes? Also Yes*

Lets draw a parrallel between Fundamentals:
A container is inside a pod. a pod is inside a replicaset. a replicaset is inside a deployment.
Well similarly:
A ClusterIP Service is part of a NodePort Service. A NodePort Service is Part of a Load Balancer Service.
