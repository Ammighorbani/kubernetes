## Service use when you want to connect two applications together in network layer

### 1- How svc or service work
**In svc we have a workflow and some other things, you have an application and you application need to connect to mariadb pods, actually you can make mariadb pods ip static, here we use svc and we connect to svc and svc will connect to `endpoint/ep` and in ep we have whole pods ip addresses but in fact we connect to coredns and coredns will give us a url and that url pointed to our svc and svc can connect to ep, svc is not a pod and it's ip is static in differente range of our pods untill you remove svc, we have differente models of svc, first one is `default cluster ip` and here it's mean your svc give an ip in range of your cluster network and in this case you can't call that ip of your host only your can call it when you have an access to your cluster and it's internal, second one is `node port` in this case you will expose a port on your server and you can connect to that port and send your request to your svc, third one is `load balancer` and in this case you won't use it in your normal cluster but if you give kaas you will see it**

---

### 2- You can create service or svc
```yml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: my-ns
  labels:
    app: nginx
spec:
  #type: NodePort
  type: ClusterIP
  selector:
    app: nginx
  ports:
    - name: http
      port: 80
      targetPort: 80
```

---

### 3- You can get your svc
```bash
kubectl get svc -n my-ns
kubectl get svc -n [NS-NAME]
```

---

### 4- You can get your endpoint or ep
```bash
kubectl get ep -n my-ns
kubectl get ep -n [NS-NAME]
```

---

### 5- How to connect a pod to service
**You need to use same metadata like ns and labels between each other, you can see better below**
```yml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: my-ns
  labels:
    app: nginx
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
    - name: http
      port: 80
      targetPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: my-ns
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
```

#### Note: You can use port forward to access to your pod in `ClusterIP` mode

---

### 6- How to port forward
```bash
kubectl port-forward -n my-ns service/nginx 80 80 --address=0.0.0.0

kubectl port-forward -n [NS-NAME] service/[SVC-NAME] [PORT] [PORT] --address=[IP]
```

---

### 7- How to connect from a pod to another pod
**You neec to use this pattern `[SVC-NAME].[NS-NAME].svc.[CLUSTER-NAME]` look like below**
```bash
nginx.my-ns.svc.cluster.local
```

#### Note: You can create a alpine image pod and exec into it and use `nslookup nginx.my-ns.svc.cluster.local` and you can see the resault or connect with this to your nginx using curl