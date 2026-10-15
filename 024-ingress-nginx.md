## Ingress nginx is a load balancer for kubernetes cluster and you need to deploy it into your own cluster to connect from outside to your internal services

#### Note: We have difference ingress controller but in avrage 90% using nginx ingress controller

### 1- Deploy nginx ingress controller using helm
**You can use [kubernetes documentation](https://kubernetes.github.io/ingress-nginx/deploy/)**

**Add repository**
```bash
helm repo add nginx-ingress https://kubernetes.github.io/nginx-ingress/
```

**Update repository**
```bash
helm repo update
```

**Save helm chart**
```bash
helm pull ingress-nginx ingress-nginx
```

**Unarchive ingress**
```bash
tar -xzvf ingress-nginx-4.15.1.tgz
```

**Save backup of values.yml**
```bash
cp values.yaml values.yaml.back
```

**Change into `ingress-nginx/values.yml`**
```bash
type: ClusterIP
hostNetwork: true
hostPort.enabled: true
```

#### Note: Change these 3 things cause we don't have load balancer and we want to open ports on our cluster and use ClusterIP type

**Deploy**
```bash
helm upgrade --install --namespace ingress-nginx --create-namespace ingress-nginx ./ingress-nginx -f ./ingress-nginx/values.yml
```

**Output**
```bash
I1015 10:51:15.392020 2564259 reflector.go:664] "Warning: watch ended with error" reflector="k8s.io/client-go@v0.37.0/tools/cache/reflector.go:343" type="rbac.authorization.k8s.io/v1, Kind=RoleBinding" err="an error on the server (\"unable to decode an event from the watch stream: context canceled\") has prevented the request from succeeding"
Release "ingress-nginx" has been upgraded. Happy Helming!
NAME: ingress-nginx
LAST DEPLOYED: Thu Oct 15 10:51:11 2026
NAMESPACE: ingress-nginx
STATUS: deployed
REVISION: 4
DESCRIPTION: Upgrade complete
TEST SUITE: None
NOTES:
The ingress-nginx controller has been installed.
Get the application URL by running these commands:
  export POD_NAME="$(kubectl get pods --namespace ingress-nginx --selector app.kubernetes.io/name=ingress-nginx,app.kubernetes.io/instance=ingress-nginx,app.kubernetes.io/component=controller --output jsonpath="{.items[0].metadata.name}")"
  kubectl port-forward --namespace ingress-nginx "${POD_NAME}" 8080:80
  echo "Visit http://127.0.0.1:8080 to access your application."

An example Ingress that makes use of the controller:
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: example
    namespace: foo
  spec:
    ingressClassName: nginx
    rules:
      - host: www.example.com
        http:
          paths:
            - pathType: Prefix
              backend:
                service:
                  name: exampleService
                  port:
                    number: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
      - hosts:
        - www.example.com
        secretName: example-tls

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
```

---

### 2- Connect from outside of cluster to services
**You can write a manifest file to write ingress config and deployment and other things you want and when you search you domain, you can see your services**
```yml
root@k8s-2:~/workspace/k8s-manifest# cat all.yml
apiVersion: v1
kind: Namespace
metadata:
  name: test-ns
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: sematec-website
  namespace: test-ns
  labels:
    name: sematec-website
    instance: sematec-website
data:
  API_URL: https://api.sematec.ir
  APP_URL: https://api.sematec.ir
---
apiVersion: v1
kind: Service
metadata:
  name: sematec-svc
  namespace: test-ns
  labels:
    name: sematec-website
    instance: sematec-website
spec:
  type: ClusterIP
  ports:
    - port: 80
      targetPort: http
      protocol: TCP
      name: http
  selector:
    name: sematec-website
    instance: sematec-website
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sematec-dp
  namespace: test-ns
  labels:
    name: sematec-website
    instance: sematec-website
spec:
  replicas: 3
  selector:
    matchLabels:
      name: sematec-website
      instance: sematec-website
  template:
    metadata:
      labels:
        name: sematec-website
        instance: sematec-website
    spec:
      containers:
        - name: sematec-website
          image: nginx
          envFrom:
            - configMapRef:
                name: sematec-website
          env:
            - name: CLIENT_URL
              value: 'https://client.sematec.ir'
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: sematec-website
  namespace: test-ns
  labels:
    name: sematec-website
    instance: sematec-website
spec:
  ingressClassName: nginx
  rules:
    - host: "sematec.ir"
      http:
        paths:
          - pathType: Prefix
            path: /
            backend:
              service:
                name: sematec-svc
                port:
                  number: 80
```