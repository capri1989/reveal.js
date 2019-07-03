# Demo

## Initial setup
Start `vagrant` vm
```
vagrant up
```

```
vagrant ssh
```

Have a look what we have

```
lsblk # 3 disks = 3 osds
kubectl get nodes
```

## Use rook to setup ceph

All CRDs for Ceph:
```
cd rook/cluster/examples/kubernetes/ceph/
kubectl create -f common.yaml
```

Swich context to `rook-ceph`
```
kubectl config set-context $(kubectl config current-context) --namespace=rook-ceph
```

Apply rbac rules for CSI deployment
```
kubectl apply -f csi/rbac/rbd/
kubectl apply -f csi/rbac/cephfs/
```

Deploy operator with CSI aktivated
```
kubectl apply -f operator-with-csi.yaml
```

Wait until eveything is deploymed (operator + CSI)
```
watch -n 1 kubectl get pods
```

Prepare cluster deployment and use disks sdb, sdc, sdd
```
vi cluster-test.yaml
>> deviceFilter: "^sd[b-d]" <<
```

Depoy the cluster
```
kubectl apply -f cluster-test.yaml
watch -n 1 kubectl get pods
kubectl logs -f rook-ceph-osd-prepare-minikube-vzdtl -c provision
```

Create toolbox (for Ceph CLI)
```
kubectl create -f toolbox.yaml
kubectl exec -it rook-ceph-tools bash
```

## CSI deployment
```
cd csi/example/rbd/
```

Create RBD pool
```
kubectl apply -f pool.yaml
```

Get a key and populate it via a secret
```
kubectl exec -it rook-ceph-operator-c7f9d796b-dzxjn ceph auth get-key client.admin
vi secret.yaml
>>
stringData:
  userID: admin
  userKey: <KEY>
<<
kubectl apply -f secret.yaml
```

Add the storage class 
```
kubectl apply -f storageclass.yaml
```

Create a PVC
```
kubectl apply -f pvc.yaml
```

Create a POD that uses the PV
```
kubectl apply -f pod.yaml
```

Some test to do
```
# in the pod
mount | grep rbd
cd /var/lib/www/html
dd if=/dev/zero of=test bs=1M count=1000
```
```
# in the tools container
ceph df 
```


