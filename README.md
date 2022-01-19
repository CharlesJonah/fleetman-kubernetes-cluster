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
