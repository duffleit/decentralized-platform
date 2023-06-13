# Decentralized Platform Demo with CAPI

Slides for more context can be found [here](https://tbd) 

## Simple Setup

```
export PORTFWD="kubectl port-forward svc/argocd-server 8080:443"

# Setup Mgmt Cluster
kind create cluster --config mgmt-cluster-config.yaml --name mgmt

# Initialize CAPI on mgmt-cluster
clusterctl init --infrastructure docker

# Show Cluster resource exists
kubectl get cluster

# Install Argo
kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Forward Port
eval ${PORTFWD}

# Apply new cluster agains argo
kubectl apply -f argo-team-a.yaml

# Show cluster
clusterctl describe cluster platform-team-a

# Show Kubeconfig
kind export kubeconfig --name platform-team-a
kind get kubeconfig --name platform-team-a
# change ~/.kubeconfig to 127.0.0.1

# Install CNI
kubectl apply -f https://docs.projectcalico.org/v3.20/manifests/calico.yaml --context kind-platform-team-a

# Create Argo Cluster
argocd login localhost:8080
argocd cluster add kind-platform-team-a

# get secrets from Lens and create argo-cluster manually

# create Kafka/Strimzi infrastructure
kubectl apply -f argo-team-a-workloads.yaml
```

### Cleanup

```
kind delete clusters mgmt team-a team-b #yolo
```