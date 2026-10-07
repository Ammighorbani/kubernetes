## Persistent volume or PV is look like LVM in linux you can create a block space with PV and it's look like your physical volume and you can give a slice of your PV and we call it PVC or persistent volume claim and it's look like your LV in LVM and you can extend it base on your PV and PVC is look like resizable volume and you can resize it in a second

### 1- Difference model of PV and PVC
`Read only many (ROX)` **:** **Mount read only on whole nodes**
`Read Write once (RWO)` **:** **Create a PV only on a specific node**
`Read Write many (RWX)` **:** **Mount read and write on whole nodes**
`Read Write once pod (RWOP)` **:** **Only read and write on a specific pod**

#### Note: You can choose for example RWX for your PV but change your PVC to ROX if you don't configure it by yourself your PVC will use PV model

---

### 2- Difference states your PV and PVC can have
`Provisioning` **:** **It's mean when you don't have that disk and kubernetes will create it for you**
`Binding` **:** **It's mean mount that disk to your pod**
`Using` **:** **After binding and mount your disk to your pod your PV or PVC will go into using state**
`Releasing` **:** **When you remove your pod**
`Reclaiming` **:** **When you create a new pod and mount it to that PVC**

---

### 3- Difference model of reaclaiming
`Retain` **:** **If you want to keep your data in PVC reclaiming model**
`Recycle` **:** **If your want to delete your data in PV reclaiming model and delete the data look like rm -rf under your path**
`Delete` **:** **If your want to delete your data in PV reclaiming model and delete the data send them to trash bin**

---

### 4- You can mount path from your pod to somewhere else like below
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: my-ns
  labels:
    app.kubernetes.io/name: nginx
    app.kubernetes.io/env: development
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: nginx
      app.kubernetes.io/env: development
  template:
    metadata:
      labels: 
        app.kubernetes.io/name: nginx
        app.kubernetes.io/env: development
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
          - containerPort: 80
          volumeMounts:
          - name: nginx-logs
          mountPath: /var/log/nginx
          - name: nginx-conf
          mountPath: /etc/nginx/conf.d
      volumes:
        - name: nginx-logs
          hostPath:  
```