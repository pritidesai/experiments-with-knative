```
curl -L https://github.com/knative/serving/releases/download/latest/istio.yaml \
  | sed 's/LoadBalancer/NodePort/' \
  | kubectl apply --filename -
```

