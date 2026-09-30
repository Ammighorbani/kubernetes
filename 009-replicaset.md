## One layer higher than pod is replicaset and you have more features than pod

### 1- Whats the difference between replicaset and pod
**With pod you only can create a single pod with any number of container you want but you could not create multiple pods, but you can create any number of pods you want, and you have some limits about configuration here, for example you can't reconfigure and change your pods images without any stop or down time and if you want to do that you need to one time remove whole your replicaset and recreate it look like pod, and replicaset will guarantee it allways will up pods base on that number you wrote into your manifest yaml file, and if you have any changes on your manifest files and apply it on your cluster, your pods won't kill or terminate and up with new version and it will wait until your pod kill or terminate and then after that your new pods will comes up base on new version and configuration, it's the important point and different with deployment**

---

### 2- You can create replicaset
```yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-app
  namespace: my-ns
  labels:
    app.kubernetes.io/name: my-app
    app.kubernetes.io/dev: development
spec:
  replicas: 3
  selector:
    matchLabels:
      app.kubernetes.io/name: my-app
  template:
    metadata:
      labels:
        app.kubernetes.io/name: my-app
    spec:
      containers:
        - name: nginx
          image: nginx
```

#### Note: You must use same labels in **spec.selector.matchLabels** and **spec.template.metadata.labels** cause when kubernetes want to create your pod need to know wich pods are for wich replica to give orchestration access, if you don't do it when you want to apply your manifest will receive an error

#### Note: In replicaset when your pod go into terminating, without any waiting your replicaset will create new pod

---

### 3- You can see your replicaset
```bash
kubectl get rs -n my-ns
kubectl get replicasets -n my-ns
kubectl get replicasets -n [NS-NAME]
```

---

### 4- You can scale your pods with zero down time
**If you change your manifest file and apply it your replicaset will scale your pods with zero downtime and if you scale down your replicaset with zero down time your replicaset will scale down**

---

### 5- You can edit your replicaset
```bash
kubectl edit -n my-ns replicaset my-app
kubectl edit -n [NS-NAME] replicaset [RS-NAME]
```

---

### 6- You can scale your replicaset with ad hock command
```bash
kubectl scale -n my-ns replicaset my-app --replicas 3

kubectl scale -n [NS-NAME] replicaset [RS-NAME] --replicas [NUMBER]
```