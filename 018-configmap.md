## Configmap
**You can add whole your configuration files look like your nginx.conf configuration file to your configmap, we have 2 differente methods to create config map, first one is you create your configmap with your cli, second one is you create your configmap with write a manifest file, sometimes like nginx.conf configuration files it's better you use cli method and when you use configmap your configuration will save into `etcd`, and we use configmap to don't mount our configuration files and tell to our deployment use that configmap**

### 1- You can create config map for your configuration files
```bash
kubectl -n my-ns create configmap nginx-conf --from-file=nginx.conf

kubectl -n [NS-NAME] create configmap [CM-NAME] --from-file=[FILE-NAME]
```

---

### 2- You can see your configmaps
```bash
kubectl get cm -n my-ns

kubectl get cm -n [NS-NAME]
```

---

### 3- You can edit your configmaps
```bash
kubectl edit cm -n my-ns nginx-conf

kubectl edit cm -n [NS-NAME] [CM-NAME]
```

---

### 4- You can create configmap with manifest file
```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-conf
  namespace: my-ns
data:
  nginx.conf: |2

    user  nginx;
    worker_processes  auto;

    error_log  /var/log/nginx/error.log notice;
    pid        /run/nginx.pid;


    events {
        worker_connections  1024;
    }


    http {
        include       /etc/nginx/mime.types;
        default_type  application/octet-stream;

        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';

        access_log  /var/log/nginx/access.log  main;

        sendfile        on;
        #tcp_nopush     on;

        keepalive_timeout  65;

        #gzip  on;

        include /etc/nginx/conf.d/*.conf;
    }
```

---

### 5- Use configmap
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
  replicas: 2
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
          - name: nginx-conf
            mountPath: /etc/nginx/nginx.conf
            subPath: nginx.conf
      volumes:
        - name: nginx-conf
          configMap:
            name: nginx-conf
```

---

### 6- You can remove your configmap
```bash
kubectl cm delete -n my-ns nginx-conf

kubectl cm delete -n [NS-NAME] [CM-NAME]
```