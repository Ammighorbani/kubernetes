## Cronjob is look like linux cronjob in kubernetes

### 1- You can create cronjob
```yml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: mycj
  namespace: my-ns
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: mycronjob
              image: busybox
              command:
                - /bin/sh
                - -c
                - date; echo hellow from kubernetes cluster
          restartPolicy: Never
```

---

### 2- You can get your cronjob
```bash
kubectl get cronjob.batch -n my-ns
kubectl get cronjob.batch -n [NS-NAME]
```

---

### 3- You can get cronjob log
```bash
kubectl logs -n my-ns mycronjob-29849332-74dmb
kubectl logs -n [NS-NAME] [CJ-PD-NAME]
```

---

### 4- You can edit your cronjob
```bash
kubectl edit cronjobs.bath -n my-ns mycronjob
kubectl edit cronjobs.bath -n [NS-NAME] [CJ-NAME]
```

---

### 5- You can remove cronjob
```bash
kubectl delete cronjobs.batch -n my-ns mycronjob
kubectl delete cronjobs.batch -n [NS-NAME] [CJ-NAME]
```