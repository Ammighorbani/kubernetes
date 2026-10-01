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