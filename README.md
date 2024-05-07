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