setting up cluster:


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