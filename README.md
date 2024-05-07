# CKAD-hands-on-demo
Preparing effectively for the Certified Kubernetes Application Developer (CKAD) certification involves a combination of hands-on exercises and focused demo rehearsals. This certification assesses your ability to design, deploy, and manage applications on Kubernetes clusters efficiently.



### Create a new pod with the nginx image. Showing all containers running and a Readystatus

<details><summary>show</summary>
<p>

```bash
kubectl run nginx --image nginx
```

```bash
 kubectl get pods -o=jsonpath='{range .items[*]}{range .status.containerStatuses[?(@.state.running)]}{"\tContainer Name: "}{.name}{"\tRunning: "}{.state.running}{"\tReadyStatus: "}{.ready}{"\n"}{end}{end}'
```

### Create a new service exposing the pod as anodePort, which presents a working webserver configured in the previousstep

<details><summary>show</summary>
<p>

```bash
kubectl create service nodeport svc --tcp=5678:80 --dry-run -o yaml > svc.yaml
```


```bash
vi svc.yaml
```

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: svc
  name: svc
spec:
  ports:
  - name: 5678-80
    port: 5678
    protocol: TCP
    targetPort: 80
  selector:
    run: nginx // Add run: nginx selector 
  type: NodePort
status:
  loadBalancer: {}
```

```bash
>ctr:x!
```

```bash
kubectl apply -f svc.yaml
```

```bash
kubectl get svc svc -o wide
```

```bash
curl 10.110.220.208:5678
```

</p>
</details>