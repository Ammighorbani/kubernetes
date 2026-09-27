## You can write manifest file to avoid create your pods and other things adhock

### 1- You can write a manifest file to create namespace
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

#### Note: You can reconfigure your pod with apply it again

---

### 2- You can delete your things you have created with manifest file
```bash
kubectl delete -f ns.yml
```

---

### 3- You can create allways up manifest
**If you put a manifest pod in `/etc/kubernetes/manifests/` after reboot or other things your manifest will run and your pods will be up and we call it `static pod`**