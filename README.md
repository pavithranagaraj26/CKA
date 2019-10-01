# CKA

Lab 01
- Create a deployment named webapp in the web namespace and verify connectivity.
  Use the following command to create a namespace named web:
`kubectl create ns web`
- Use the following command to create a deployment named webapp:
`kubectl run webapp --image=linuxacademycontent/podofminerva:latest --port=80 --replicas=3 -n web`
- Create a service named web-service and forward traffic from the pods.
- Use the following command to get the IP address of a pod that’s a part of the deployment:
`kubectl get po -o wide -n web`
- Use the following command to create a temporary pod with a shell to its container:
`kubectl run busybox --image=busybox --rm -it --restart=Never -- sh`
- Use the following command (from the container’s shell) to send a request to the web pod:
`wget -O- <pod_ip_address>:80`
- Use the following command to create the YAML for the service named web-service:
`kubectl expose deployment/webapp --port=80 --target-port=80 --type=NodePort -n web --dry-run -o yaml > web-service.yaml`
- Use Vim to add the namespace and the NodePort to the YAML:
`vim web-service.yaml`
- Change the name to web-service, add the namespace web, and add nodePort: 30080.
- Use the following command to create the service:
`kubectl apply -f web-service.yaml`
- Use the following command to verify that the service is responding on the correct port:
`curl localhost:30080`
- Use the following command to modify the deployment:
`kubectl edit deploy webapp -n web`
- Add the liveness probe and the readiness probe:
``` 
livenessProbe
  httpGet:
    path: /healthz
    port: 8081
readinessProbe:
  httpGet:
    path: /
    port: 80
 ```
- Use the following command to check if the pods are running:
`kubectl get po -n web`
- Use the following command to check if the probes were added to the pods:
`kubectl get po <pod_name> -o yaml -n web --export`
