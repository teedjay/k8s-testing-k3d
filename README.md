# How to start k3d kubernetes cluster

- have Docker running
- brew install k3d
- brew install kubeclt

## Create a small cluster (name will be `k3s-default` if not specified)
Will start ingress loadbalancer Traefik on port 80, expose it on 10080 on localhost.
```
k3d cluster create -p "10080:80@loadbalancer"
k3d kubeconfig merge k3s-default --switch-context
```

## Start the nginx demo
Will start nginx and expose it using ingress and inrgess rewrite rules.
```
kubectl apply -f pod.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```