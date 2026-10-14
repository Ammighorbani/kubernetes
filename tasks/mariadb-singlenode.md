## Deploy single node mariadb
**We need a single node mariadb database and deploy it with manifest file with these resources,one longhorn storage class, one statefulset for mariadb, one service, and if you want one configmap for mariadb credentials, and then we need a alpine linux to connect from alpine to mariadb database**

### 1- longhorn manifest file
```
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.13.0/deploy/longhorn.yaml
```

### 2- task manidest file
```bash
apiVersion: v1
kind: Service
metadata:
  name: mariadb-svc
  namespace: my-ns
  labels:
    app: mariadb
spec:
  ports:
    - name: mariadb
      port: 3306
  clusterIP: None
  selector:
    app: mariadb
---
apiVersion: v1
kind: Secret
metadata:
  name: mariadb-secret
  namespace: my-ns
type: Opaque
data:
  mariadb-root-password: QW1taWdob3JiYW5pMTI=
  mariadb-database: YW1taWRi
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mariadb-single-node
  namespace: my-ns
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mariadb
  serviceName: "mariadb-svc"
  template:
    metadata:
      labels:
        app: mariadb
    spec:
      containers:
        - name: mariadb
          image: mariadb
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mariadb-secret
                  key: mariadb-root-password

            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: mariadb-secret
                  key: mariadb-database
          ports:
            - containerPort: 3306
              name: db
          volumeMounts:
            - mountPath: /var/lib/mysql
              name: db
  volumeClaimTemplates:
    - metadata:
        name: db
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 2Gi
        storageClassName: "longhorn"
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ubuntu
  namespace: my-ns
  labels:
    app: ubuntu-mariadb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ubuntu-mariadb
  template:
    metadata:
      labels:
        app: ubuntu-mariadb
    spec:
      containers:
        - name: ubuntu-mariadb
          image: alpine
          env:
            - name: MYSQL_PWD
              valueFrom:
                secretKeyRef:
                  name: mariadb-secret
                  key: mariadb-root-password
          command: ["sh", "-c"]
          args:
            - |
              apk add --no-cache mariadb-client

              echo "Waiting for MariaDB to become ready..."
              until mariadb-admin ping -h mariadb-svc -u root --silent; do
                echo "MariaDB is unavailable - sleeping 2s"
                sleep 2
              done

              echo "Mariadb is up..."
              mariadb -h mariadb-svc -u root -e "
                CREATE DATABASE IF NOT EXISTS ammidb;
                USE ammidb;
                CREATE TABLE IF NOT EXISTS info (
                  id INT AUTO_INCREMENT PRIMARY KEY,
                  ayo BOOLEAN
                );
                INSERT INTO info (ayo) VALUES (true);
              "

              while true; do
                echo "Operation done!"
                sleep 5
              done
```