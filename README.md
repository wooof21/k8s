# k8s
kubernetes


local cluster: kind - https://kind.sigs.k8s.io/docs/user/quick-start/#creating-a-cluster


commands:
* get all cluster nodes: `kubectl get nodes`
* get a list of Pods running: `kubectl get pod`
    * or any resources running: `kubectl get <resource>`
    * with a little more info about pod: `kubectl get pod -o wide`
* get detailed description of all pod: `kubectl describe pod`
    * description of a particular pod: `kubectl describe pod <pod_name>`
* get log of a pod: `kubectl logs <pod_name>`
    * when have multiple containers: `kubectl logs <pod_name>` default to the first container
    * to check a particular container: `kubectl logs <pod_name> -c <container_name>`
* get replicaSet: `kubectl get replicaSet` or `kubectl get rs`

* get all run resources: `kubectl get all`

* get namespace: `kubectl get namespace` or `kubectl get ns`
    * create namespace: `kubectl create ns <namespace_name>`
    * create resource in namespace: `kubectl apply -f <yaml file> -n <namespace>`
* delete namespace and all resources in namespace: `kubectl delete ns <namespace>`

* port forward with optional namespace: `kubectl port-forward <resources>/<resource_name> <machine_port:service_port> -n <namespace>`   
    * resources: service(or svc) || deployment || pod
    * `kubectl port-forward svc/my-service 8080:80 -n dev`
    * `kubectl port-forward my-pod 8080:80 -n qa`
    * `kubectl port-forward deploy/order-service-deploy 8080:80 -n uat`
    * namespace can also be included in yaml as metadata:
        ```
        metadata:
            name: order-service-deploy
            namespace: dev
        ```

## Probes
* Live/Liveness: is pod alive? 
* Ready/Readiness: is pod ready to serve the request?
    * `Live but not ready`: one of the dependent service(database) is down, the app is Live,
        but not in a position to serve the request


## Auto-scale
* See resource consumption for nodes or pods: `kubectl top <node>|<pod>`
    * Install the Metrics Server: `kubectl apply -f .\09-auto-scale\metrics-server-install\metrics-server.yaml`
        * Validate: `kubectl get pod -n kube-system` -> the metrics-server pod was added
        * `kubectl top node`: list all nodes resources utilization
