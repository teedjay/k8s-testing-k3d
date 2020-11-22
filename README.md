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
Will start nginx and expose index.html with a bootstrap welcome page using ingress and inrgess rewrite rules.
```
kubectl apply -f pod.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

## Testing the ingress routing
This should work in your browser with images etc : http://localhost:10080/web/

Here are some other rewrites that will "miss" the correct entrypoint.
```
# should be 404 from traefik ingress
curl -i :10080/

# using Prefix match, all will hit nginx
curl -i http://localhost:10080/web
curl -i http://localhost:10080/web123
curl -i http://localhost:10080/web/

# using Exact match, first should be 404 from ingress ingress, second should hit nginx
curl -i http://localhost:10080/copy
curl -i http://localhost:10080/copy/