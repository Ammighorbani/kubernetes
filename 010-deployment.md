## One layer higher than replicaset is deployment and you have more features than replicaset

### 1- Whats the differente between deployment and replicaset
**Most of the things is look like replicaset but the most important differente is you can reconfigure your pods with zero down time and without any wait**

---

### 2- You can create deployment
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
  namespace: my-ns
  labels:
    app.kubernetes.io/name: myapp
    app.kubernetes.io/env: development
spec:
  replicas: 4
  selector:
    matchLabels:
      app.kubernetes.io/name: myapp
  template:
    metadata:
      labels:
        app.kubernetes.io/name: myapp
    spec:
      containers:
        - name: nginx
          image: nginx
```

---

### 3- You can get your deployment
```bash
kubectl get deploy -n my-ns
kubectl get deploy -n [NS-NAME]
```

---

### 4- You can edit your deployment
```bash
kubectl edit -n my-ns deployment myapp
kubectl edit -n [NS-NAME] deployment [DP-NAME]
```

---

### 7- You can scale your deployment
```bash
kubectl scale -n my-ns deployment myapp --replicas 6

kubectl scale -n [NS-NAME] deployment [DP-NAME] --replicas [NUMBER]
```

---

### 8- You can delete your deployment
```bash
kubectl delete -n my-ns deployment myapp
kubectl delete -n [NS-NAME] deployment [DP-NAME]

kubectl delete -f dp.yml
kubectl delete -f [DP-MANIFEST].yml
```

---

### 9- You can rollout/rollback your deployment
**You can rollback your deployment to each last versions but you need to know how many version your deployment have and know information about each version**

**See rollout history:**
```bash
kubectl rollout history -n my-ns deployment myapp

kubectl rollout history -n [NS-NAME] deployment [DP-NAME]
```

**Check each revision information:**
```bash
kubectl rollout history -n my-ns deployment myapp --revision 1

kubectl rollout history -n [NS-NAME] deployment [DP-NAME] --revision [NUMBER]
```

**Rolle back:**
```bash
kubectl rollout undo -n my-ns deployment myapp --to-revision 1

kubectl rollout undo -n [NS-NAME] deployment [DP-NUMBER] --to-revision [NUMBER]
```

---

### 10- You can autoscale your deployment
```bash
kubectl autoscale -n my-ns deployment myapp --cpu=20 --min=4 --max=10

kubectl autoscale -n [NS-NAME] deployment [DP-NAME] --cpu=[PERCENT-NUMBER] --min=[NUMBER] --max=[NUMBER]
```

---

### 11- You can see your hpa
```bash
kubectl get hpa -n my-ns
kubectl get hpa -n [NS-NAME]
```