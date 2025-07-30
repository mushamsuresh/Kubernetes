## Deoployement commands
### Create/update from manifest
kubectl apply -f deployment.yaml

### Scale to 5 replicas
kubectl scale deploy/web-frontend --replicas=5

### Roll out a new image
kubectl set image deploy/web-frontend nginx=nginx:1.30

### View rollout history
kubectl rollout history deploy/web-frontend

### Roll back to previous revision
kubectl rollout undo deploy/web-frontend

### kubectl command to create/update resources from a manifest or delete it:
kubectl delete -f my-deployment.yaml
## Replica commands
### Create from YAML
kubectl apply -f replicaset.yaml

### View ReplicaSets
kubectl get rs

### Describe a ReplicaSet
kubectl describe rs my-replicaset

### Scale it manually
kubectl scale rs my-replicaset --replicas=5

