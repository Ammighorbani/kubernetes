## Helm is kubernetes package manager and with helm chart you don't need to write manifest files like before and you can use helm chart to deploy your services

### 1- Install helm
**You can use difference ways to install your helm but easiest way to install is using helm script file**
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
chmod 700 get_helm.sh
./get_helm.sh
```

#### Note: Before installation always check [helm website](https://helm.sh)

---

### 2- Helm repository
**Look like apt package manger you need to add each things you want to use repository for example `kube-prometheus-stack` repository and you can find helm information about each project into [artifacthub](https://artifacthub.io)**


---

### 3- Add helm repository
```bash
helm repo add [REPO-NAME] [REPO-URL]

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

---

### 4- Update helm repository
```bash
helm repo update
```

---

### 5- Check helm repositories
```bash
helm repo list
```

---

### 6- Search into repository list
```bash
helm search repo ingress-nginx
```

---

### 7- Install helm charts
```bash
helm pull [REPO-NAME]/[CHART-NAME]

helm pull ingress-nginx/ingress-nginx
```

---

### 8- Deploy kube-prometheus-stack with helm
**You can see information and helm repository [here](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack?modal=install)**

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

#### Note: You need to update your helm repositories `helm repo update`

**Installation**
```bash
helm install monitoring prometheus-community/kube-prometheus-stack --version 92.3.1 -n monitoring
```

#### Note: `monitoring` name in our command is look like a tag you use for your deployment and in future you can rallback or delete your service with this name and you can change it to each name you want and you can choose to deploy into wich namespace you want to deploy

**Output**
```bash
NAME: monitoring
LAST DEPLOYED: Wed Oct 14 01:04:32 2026
NAMESPACE: monitoring
STATUS: deployed
REVISION: 1
DESCRIPTION: Install complete
NOTES:
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "release=monitoring"

Get Grafana 'admin' user password by running:

  kubectl --namespace monitoring get secrets monitoring-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo

Access Grafana local instance:

  export POD_NAME=$(kubectl --namespace monitoring get pod -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=monitoring" -oname)
  kubectl --namespace monitoring port-forward $POD_NAME 3000

Get your grafana admin user password by running:

  kubectl get secret --namespace monitoring -l app.kubernetes.io/component=admin-secret -o jsonpath="{.items[0].data.admin-password}" | base64 --decode ; echo


Visit https://github.com/prometheus-operator/kube-prometheus for instructions on how to create & configure Alertmanager and Prometheus instances using the Operator.
```

---

### 9- Uninstall your recent helm installed services
```bash
helm uninstall -n [NS-NAME] [RELEASE-NAME]

helm uninstall -n ingress-nginx ingress-nginx
```

---

### 10- Whole list of releases
```bash
helm list -n [NS-NAME]

helm list -n ingress-nginx
```