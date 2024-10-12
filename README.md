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
</p>
</details>

### Create a new service exposing the pod as a nodePort, which presents a working webserver configured in the previousstep

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
    run: nginx # Add run: nginx selector 
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
NAME   TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE   SELECTOR
svc    NodePort   10.110.220.208   <none>        5678:31030/TCP   20m   run=nginx
```

```bash
curl 10.110.220.208:5678 // from the same vm
```
```bash
curl 10.110.220.208:31030 // from the your local machine or navigator
```

</p>
</details>

### Update the pod to run thenginx:1.11-alpineimage and re-verify you can view the webserver via a nodePort
<details><summary>show</summary>
<p>

```bash
kubectl edit pod nginx
```

```yaml
 apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2024-05-07T20:21:50Z"
  labels:
    run: nginx
  name: nginx
  namespace: default
  resourceVersion: "141816"
  uid: a7e2720e-3a8e-46bc-af6f-93e1d1b55a88
spec:
  containers:
  - image: nginx:1.11-alpine # change here
    imagePullPolicy: Always

```

```bash
>ctr:x!
```

```bash
kubectl get pod nginx
```

```bash
NAME    READY   STATUS    RESTARTS        AGE
nginx   1/1     Running   1 (5m53s ago)   40m
```


```bash
kubectl describe pod nginx
```

```bash
Name:             nginx
Namespace:        default
Priority:         0
Service Account:  default
Node:             ip-172-31-28-164/172.31.28.164
Start Time:       Tue, 07 May 2024 20:21:50 +0000
Labels:           run=nginx
Annotations:      <none>
Status:           Running
IP:               10.0.2.145
IPs:
  IP:  10.0.2.145
Containers:
  nginx:
    Container ID:   containerd://f9d93fb6dbd4d69d7e9de30747591d55feb95b31cd6da55ab8fcd1b8a2c6047b
    Image:          nginx:1.11-alpine // => should be the new nginx version

```

```bash
curl 10.110.220.208:5678 // from the same vm
```
```bash
curl 10.110.220.208:31030 // from the your local machine or navigator
```

```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

</p>
</details>

<p>


### Create a job which will run a container which sleeps for three seconds then stops

<details><summary>show</summary>
<p>

```bash
vim job.yaml
```

```yaml
kubectapiVersion: batch/v1
kind: Job
metadata:
  name: my-job
spec:
  template:
    spec:
      containers:
      - name: my-container
        image: busybox
        command: ["sleep", "5"]
      restartPolicy: Never
```
</p>
</details>


### Deploy sample Pod with below configuration using kubectl.

Container Image: nginx:1.18

Pod Name: nginx-pod

<details><summary>show</summary>
<p>

```bash

bach@ip-172-31-23-202:~$ kubectl run nginx-pod --image nginx:1.18
pod/nginx-pod created

```

</p>
</details>


###  Display the nginx-pod and ensure nginx-pd is Running state.

<details><summary>show</summary>
<p>
```bash

bach@ip-172-31-23-202:~$ kubectl get pod | grep nginx-pod
```

```json

nginx-pod                                  1/1     Running            0                6s

```
</p>
</details>