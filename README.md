# Домашнее задание к занятию "12.5 Сетевые решения CNI"

## Задание 1: установить в кластер CNI плагин Calico


### DenyPolicy.yml
```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: default-deny-ingress
spec:
  podSelector: {}
  policyTypes:
    - Ingress
```

### NWPolicy.yml

```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: testcurl
spec:
  podSelector:
    matchLabels:
      app: hello-node
  ingress:
    - from:
      - podSelector:
          matchLabels:
            app: multitool
  policyTypes:
  - Ingress
```

### Проверка доступности hello-node
```
rimm@cp1:~$ kubectl apply -f NWPolicy.yml
networkpolicy.networking.k8s.io/testcurl created
rimm@cp1:~$ kubectl exec -it multitool-546689b6bb-4c65l -- bash
bash-5.1# curl hello-node:8080
CLIENT VALUES:
client_address=10.233.90.2
command=GET
real path=/
query=nil
request_version=1.1
request_uri=http://hello-node:8080/

SERVER VALUES:
server_version=nginx: 1.10.0 - lua: 10001

HEADERS RECEIVED:
accept=*/*
host=hello-node:8080
user-agent=curl/7.79.1
BODY:
```


## Задание 2: изучить, что запущено по умолчанию

### Вывод calicoctl get node
```
rimm@cp1:~$ calicoctl get node
NAME
cp1
node1
```
### Вывод calicoctl get ippool
```
rimm@cp1:~$ calicoctl get ippool
NAME           CIDR             SELECTOR
default-pool   10.233.64.0/18   all()
```
### Вывод calicoctl get profile
```
rimm@cp1:~$ calicoctl get profile
NAME
projectcalico-default-allow
kns.default
kns.kube-node-lease
kns.kube-public
kns.kube-system
ksa.default.default
ksa.kube-node-lease.default
ksa.kube-public.default
ksa.kube-system.attachdetach-controller
ksa.kube-system.bootstrap-signer
ksa.kube-system.calico-kube-controllers
ksa.kube-system.calico-node
ksa.kube-system.certificate-controller
ksa.kube-system.clusterrole-aggregation-controller
ksa.kube-system.coredns
ksa.kube-system.cronjob-controller
ksa.kube-system.daemon-set-controller
ksa.kube-system.default
ksa.kube-system.deployment-controller
ksa.kube-system.disruption-controller
ksa.kube-system.dns-autoscaler
ksa.kube-system.endpoint-controller
ksa.kube-system.endpointslice-controller
ksa.kube-system.endpointslicemirroring-controller
ksa.kube-system.ephemeral-volume-controller
ksa.kube-system.expand-controller
ksa.kube-system.generic-garbage-collector
ksa.kube-system.horizontal-pod-autoscaler
ksa.kube-system.job-controller
ksa.kube-system.kube-proxy
ksa.kube-system.namespace-controller
ksa.kube-system.node-controller
ksa.kube-system.nodelocaldns
ksa.kube-system.persistent-volume-binder
ksa.kube-system.pod-garbage-collector
ksa.kube-system.pv-protection-controller
ksa.kube-system.pvc-protection-controller
ksa.kube-system.replicaset-controller
ksa.kube-system.replication-controller
ksa.kube-system.resourcequota-controller
ksa.kube-system.root-ca-cert-publisher
ksa.kube-system.service-account-controller
ksa.kube-system.service-controller
ksa.kube-system.statefulset-controller
ksa.kube-system.token-cleaner
ksa.kube-system.ttl-after-finished-controller
ksa.kube-system.ttl-controller
```
