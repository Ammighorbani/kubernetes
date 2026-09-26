## We using flannel network addons

### 1- Initiate master node for cluster
```bash
kubeadm init --api-server-advertise-address [YOUR-IP] --pod-network-cidr [YOUR-NETWORK-IP-RANGE]
```

```bash
kubeadm init --api-server-advertise-address 192.168.10.50 --pod-network-cidr 10.244.0.0/16
```

#### Note: For multimaster kubernetes cluster you need to use --upload-certs and you need VIP load balancer to connect other masters

---

### 2- Information logs after successful cluster initialize
`/etc/kubernetes/pki` **:** **Here you can find whole your kubernetes cluster certificates**
`/etc/kubernetes` **:** **Kubeconfig configuration, admin.conf, super-admin.conf, ...**
`admin.conf` **:** **Most important configuration file and you can connect to your cluster with this configuration**
`super-admin.conf` **:** **It's look like admin.conf but it's look like you create another user and then add it to sudo**
`beginig pods` **:** **Your kubernetes will create builtin services like kube-apiserver, kube-etcd,... pods**
`namespaces` **:** **Creating required default namespaces**
`kubelet configurations` **:** **You will see some one liner shell scripts to configure your kubelet to connect to cluster**
`workers token` **:** **You will see some one liner shell scripts and token to join your workers**

---

### 3- Join workers to cluster
```bash
kubeadm join [API-SERVER-IP]:6443 --token [TOKEN] --discovery-token-ca-cert-hash sha256:[hash]
```

---

### 4- See your joined nodes
```bash
kubectl get nodes
```

---

### 5- Create flannel network addon
```bash
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```

---

### 6- You can make more intresting your kubectl
**You can use kubecolor open source github project to make your kubectl more intresting [see here](https://github.com/kubecolor/kubecolor)**