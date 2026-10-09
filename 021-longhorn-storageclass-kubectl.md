## Longhorn rancher storage class
**Here we will deploy longhorn storage class with kubectl you can check official longhorn website documentation [here](https://longhorn.io) and use official manifest to deploy with kubectl**

### 1- Run longhorn manifest file
```bash
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.13.0/deploy/longhorn.yaml
```

---

### 2- You can see longhorn pods
```bash
kubectl get pod -n longhorn-system
```

---

### 3- You can see longhorn svcs
```bash
kubectl get -n longhorn-system svc
```

---

### 4- You can port forward your longhorn frontend and use web ui
```bash
kubectl -n longhorn-system port-forward svc/longhorn-frontend 8080:80 --address 0.0.0.0

kubectl -n longhorn-system port-forward svc/longhorn-frontend [YOUR-PORT]:[POD-PORT] --address 0.0.0.0
```