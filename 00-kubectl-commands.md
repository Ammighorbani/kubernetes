### 1- See whole cluster pods
```bash
kubectl get pod -A
```

```bash
kubectl get pod -A -o wide
```

---

### 2- See whole cluster nodes
```bash
kubectl get nodes
```

```bash
kubectl get node -o wide
```

---

### 3- See whole cluster namespaces
```bash
kubectl get namespaces
```

```bash
kubectl get ns
```

---

### 4- See specefic namespace pods
```bash
kubectl get pod -n default
```

---

### 5- See whole api resources list
```bash
kubectl api-resources
```

---

### 6- Fix kubectl completion
**Use `kubectl completion --help` command to see configuration guidlines based on your shell**

```bash
kubectl completion bash > /etc/bash_completion.d/kubectl
```

#### Note: One time reload your shell

---

### 7- Fix kubeadm completion
**Use `kubeadm completion --help` command to see configuration guidlines based on your shell**

```bash
kubeadm completion bash > /etc/bash_completion.d/kubeadm
```

#### Note: One time reload your shell

---

### 8- You can fallow your commands
```bash
kubectl get pod -n my-ns -w
```

---

### 9- You can see your nodes labels
```bash
kubectl get nodes --show-labels
```

---

### 10- You can create new labels for your nodes
```bash
kubectl label nodes [NODE-NAME] [LABEL]

kubectl label nodes k8s-3 kubernetes.io/disk=ssd
```

---

### 11- You can copy something from your pod to your cluster
```bash
kubectl cp -n my-ns nginx-5720497ksf-497bdv:/etc/nginx/nginx.conf nginx.conf

kubectl cp -n [NS-NAME] [POD-NAME] [DEST-NAME]
```

---

### 12- You can see whole events or problems
```bash
kubectl events -n my-ns

kubectl events -n [NS-NAME]
```