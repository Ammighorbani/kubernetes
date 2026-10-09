## Statefulset or STS

### 1- You can create STS
```yml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: simple-stateful-set
spec:
  replicas: 3  # the default is 1
  selector:
    matchLabels:
      app: nginx  # has to match .spec.template.metadata.labels
  serviceName: "nginx"
  template:
    metadata:
      labels:
        app: nginx  # has to match .spec.selector.matchLabels
    spec:
      terminationGracePeriodSeconds: 10
      containers:
        - image: nginx
          name: nginx
          ports:
            - containerPort: 80
              name: web
          volumeMounts:
            - mountPath: /usr/share/nginx/html
              name: www
  volumeClaimTemplates:
    - metadata:
        name: www
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi
        storageClassName: "my-storage-class"
```

#### Note: You can connect your STS to longhorn with editing storageClassName to longhorn

---

### 2- You can get your STS
```bash
kubectl get -n my-ns statefulset simple-stateful-set
```

---

### 3- You can see your STS PVC
```bash
kubectl get pvc -n my-ns
```