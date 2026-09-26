## You can manage and categorize your pods with namespaces for example base on teams or base on organizations

### 1- You can see your namespaces
```bash
kubectl get namespaces
```

```bash
kubectl get ns
```

---

### 2- You can create your own namespaces

```bash
kubectl create namespace dev
```

#### Note: Whole around the world `kube-` is reserved and it's better to don't create a namespace with it

#### Note: If you don't dictate to your pod it namespace, it will create into the `default` namespace

---

### 3- You can remove namespaces
```bash
kubectl delete ns dev
```

---

### 4- You can create namespaces with manifest using yaml, yml files
```yml
apiVersion: v1
kind: Namespace
metadata:
  name: my-ns
```

#### Note: Each manifest file must have `apiVersion`, `kind`, `metadata` and `spec` but you can don't use

**Apply**
```bash
kubectl apply -f ns.myl
```

--- 

### 5- You can delete your things you have created with manifest file
```bash
kubectl delete -f ns.yml
```