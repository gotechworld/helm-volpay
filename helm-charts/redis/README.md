
# Bootstrapping REDIS in AKS. How to connect on it?

To get your password run:

```export REDIS_PASSWORD=$(kubectl get secret --namespace sharedservices redis -o jsonpath="{.data.redis-password}" | base64 -d)```

```echo $REDIS_PASSWORD```		
	
### To connect to your Redis&reg; server:

1. __Run a Redis&reg; pod that you can use as a client:__

   ```kubectl run --namespace sharedservices redis-client --restart='Never'  --env REDIS_PASSWORD=$REDIS_PASSWORD  --image docker.io/bitnami/redis:7.0.9-debian-11-r1 --command -- sleep infinity```

   __Use the following command to attach to the pod:__

   ```kubectl exec --tty -i redis-client --namespace sharedservices -- bash```

2. __Connect using the Redis&reg; CLI:__

   ```REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h redis -p 6379 # Master access```

   ```REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h redis -p 26379 # Sentinel access```

__To connect to your database from outside the cluster execute the following commands:__

```kubectl port-forward --namespace sharedservices svc/redis 6379:6379 &```

```REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h 127.0.0.1 -p 6379```

3. __Visualize the persistence storage:__

```kubectl get pvc --namespace sharedservices```