# Bootstrapping MONGODB in AKS. How to connect on it?
__MongoDB&reg; can be accessed on the following DNS name(s) and ports from within your cluster:__

   ```mongodb.sharedservices.svc.cluster.local```

__To get the root password run:__

 ```export MONGODB_ROOT_PASSWORD=$(kubectl get secret --namespace sharedservices mongodb -o jsonpath="{.data.mongodb-root-password}" | base64 -d)```

```echo $MONGODB_ROOT_PASSWORD```


__To connect to your database, create a MongoDB&reg; client container:__

   ```kubectl run --namespace sharedservices mongodb-client --rm --tty -i --restart='Never' --env="MONGODB_ROOT_PASSWORD=$MONGODB_ROOT_PASSWORD" --image docker.io/bitnami/mongodb:6.0.4-debian-11-r14 --command -- bash```

__Then, run the following command:__

```mongosh admin --host "mongodb" --authenticationDatabase admin -u root -p $MONGODB_ROOT_PASSWORD```

__To connect to your database from outside the cluster execute the following commands:__

```kubectl port-forward --namespace sharedservices svc/mongodb 27017:27017 &```

```mongosh --host 127.0.0.1 --authenticationDatabase admin -p $MONGODB_ROOT_PASSWORD```