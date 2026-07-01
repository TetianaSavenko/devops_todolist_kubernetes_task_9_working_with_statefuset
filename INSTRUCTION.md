# Validation Instructions

## Prerequisites
- `kind` (>= 0.20) installed
- `kubectl` installed
- `Docker` installed

## Deployment

```bash
chmod +x bootstrap.sh
./bootstrap.sh


## StatefulSet Check


```bash
# View all resources in the mysql namespace
kubectl get all -n mysql

# Verify that 3 replicas are Running
kubectl get pods -n mysql
# Expected Result:
# NAME READY STATUS RESTARTS AGE
# mysql-0 1/1 Running 0 ...
# mysql-1 1/1 Running 0 ...
# mysql-2 1/1 Running 0 ...
# Check PVC
kubectl get pvc -n mysql
# Check headless Service
kubectl get svc -n mysql
```

## Check connectivity to DB

```bash
# Connect to mysql-0
kubectl exec -it mysql-0 -n mysql -- mysql -u root -p
# Enter rootpassword

# Check the DB
SHOW DATABASES;
USE tododb;
SHOW TABLES;
```

## Verify the application

```bash
# Check the application pods
kubectl get pods
# View logs
kubectl logs deployment/todolist

# Port-forward for local test
kubectl port-forward deployment/todolist 8000:8000
# Open http://localhost:8000
```

## Verify Probes

```bash
# View events and probe status
kubectl describe pod mysql-0 -n mysql | grep -A5 "Liveness\|Readiness"
```

## Cleanup

```bash
kind delete cluster
```