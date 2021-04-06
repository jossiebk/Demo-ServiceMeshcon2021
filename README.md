# Demo-ServiceMeshcon2021

## Cluster creation
```
gcloud config set project yourproject
gcloud config set compute/zone us-central1-a

gcloud container clusters create chaos --num-nodes=2 --tags=allin,allout --enable-legacy-authorization --enable-basic-auth --issue-client-certificate --machine-type=n1-standard-2 --no-enable-network-policy
```

## Build Demo Containers
Apache Container
```
cd apache
/bin/bash build.sh czdev
```
Client Container
```
cd client
/bin/bash build.sh czdev
```

## Install Linkerd
Install Linkerd
```
curl -sL run.linkerd.io/install | sh
linkerd check --pre

linkerd install | kubectl apply -f -
linkerd check
linkerd viz install | kubectl apply -f -
linkerd viz dashboard
```

## Install Chaos Mesh
```
curl -sSL https://mirrors.chaos-mesh.org/v1.1.2/install.sh | bash
```

## Create Meshed Deployments
```
kubectl apply -f apache.yaml
```

## Install Chaos Test
```
kubectl apply -f pod-kill.yaml
kubectl delete -f pod-kill.yaml
```

## Watch for the pods 
```
kubectl get pods -n chaos-testing -w
```

## Generate Faulty Fraffic
```
kubectl apply -f faulty_traffic.yaml
kubectl delete -f faulty_traffic.yaml
```

## Access the dashboard Dashboard
```
kubectl port-forward -n chaos-testing svc/chaos-dashboard 2333:2333
```

# References
- https://linkerd.io/2.10/tasks/fault-injection/
- https://chaos-mesh.org/