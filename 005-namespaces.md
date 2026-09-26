## You can manage and categorize your pods with namespaces for example base on teams or base on organizations

### You can see your namespaces
```bash
kubectl get namespaces
```

```bash
kubectl get ns
```

---

### You can create your own namespaces

```bash
kubectl create namespace dev
```

#### Note: Whole around the world `kube-` is reserved and it's better to don't create a namespace with it

#### Note: If you don't dictate to your pod it namespace, it will create into the `default` namespace

---

