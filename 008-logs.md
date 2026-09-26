## You can use logs command to see your pods logs

### 1- You can see a pod log with single container
```bash
kubectl logs nginx

kubectl logs [POD-NAME]

kubectl logs -n my-ns ubuntu-ammighorbani

kubectl logs -n [NS-NAME] [POD-NAME]
```

---
### 2- You can see a pod log with multi containers

```bash
kubectl logs -n my-ns ubuntu-images -c ubuntu-mamadhacker

kubectl logs -n [NS-NAME] [POD-NAME] -c [CONTAINER-NAME]
```

### Note: By default you will see first container you wrote