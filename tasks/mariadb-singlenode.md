## Deploy single node mariadb

with 1 statefulset, 1 service (svc), 1 PV, 1 PVC on longhorn storage class, 1 configmap for db root password and other things

with this mariadb we need a ubuntu pod to connect from pod to that mariadb database

whole things in one manifest file