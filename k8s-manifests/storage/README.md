# Redis on Kubernetes

## Namespace

```
kubectl create ns redis
```

## Storage Class

```
kubectl get sc
NAME                    PROVISIONER          RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
azurefile               file.csi.azure.com   Delete          Immediate              true                   69d
azurefile-csi           file.csi.azure.com   Delete          Immediate              true                   69d
azurefile-csi-premium   file.csi.azure.com   Delete          Immediate              true                   69d
azurefile-premium       file.csi.azure.com   Delete          Immediate              true                   69d
default (default)       disk.csi.azure.com   Delete          WaitForFirstConsumer   true                   69d
managed                 disk.csi.azure.com   Delete          WaitForFirstConsumer   true                   69d
managed-csi             disk.csi.azure.com   Delete          WaitForFirstConsumer   true                   69d
managed-csi-premium     disk.csi.azure.com   Delete          WaitForFirstConsumer   true                   69d
managed-premium         disk.csi.azure.com   Delete          WaitForFirstConsumer   true                   69d
```

## Deployment: Redis nodes

```
cd k8s-manifests/storage/redis/
kubectl apply -f redis-configmap.yaml -n redis
kubectl apply -f redis-statefulset.yaml -n redis

kubectl -n redis get pods
kubectl -n redis get pv

kubectl -n redis logs redis-0
kubectl -n redis logs redis-1
kubectl -n redis logs redis-2
```

## Test replication status

```
kubectl -n redis exec -it redis-0 -- sh
redis-cli 
auth a-very-complex-password-here
info replication
```

## Deployment: Redis Sentinel (3 instances)

```
cd k8s-manifests/storage/sentinel
kubectl apply -f sentinel-statefulset.yaml -n redis

kubectl -n redis get pods
kubectl -n redis get pv
kubectl -n redis logs sentinel-0
```

