Apply docker secret and service account manifests

$ kubectl apply -f docker-secret.yaml
secret "basic-user-pass" created

$ kubectl get secret
NAME                  TYPE                                  DATA      AGE
basic-user-pass       kubernetes.io/basic-auth              2         21s
...

$ kubectl apply -f service-account.yaml
serviceaccount "build-bot" created

$ kubectl get serviceaccount
NAME        SECRETS   AGE
build-bot   2         24s
default     1         4d

$ kubectl apply -f service.yaml
service "app-from-source" created

$ kubectl get pods --watch

To check on the state of the service, get the service object and examine the status block:

$ kubectl get ksvc app-from-source --output yaml

$ kubectl get svc knative-ingressgateway --namespace istio-system
NAME                     TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                                                                                                                   AGE
knative-ingressgateway   LoadBalancer   10.102.222.17   <pending>     80:32380/TCP,443:32390/TCP,31400:32400/TCP,15011:32203/TCP,8060:31907/TCP,853:31782/TCP,15030:31485/TCP,15031:30843/TCP   4d

$ export IP_ADDRESS=$(kubectl get node  --output 'jsonpath={.items[0].status.addresses[0].address}'):$(kubectl get svc knative-ingressgateway --namespace istio-system   --output 'jsonpath={.spec.ports[?(@.port==80)].nodePort}')

$ echo $IP_ADDRESS
192.168.65.3:32380

$ knctl service list -n default
Services in namespace 'default'

Name             Domain                               Annotations  Conditions  Age
app-from-source  app-from-source.default.example.com  -            1 OK / 3    19m
helloworld-go    helloworld-go.default.example.com    -            1 OK / 3    4d
simple-app       simple-app.default.example.com       -            0 OK / 3    3h

3 services

Succeeded

knctl domain list
Domains

Name         Default
example.com  true

1 domains

Succeeded

knctl routes list -n default
Routes in namespace 'default'

Name             Domain                               Traffic                  Annotations  Conditions  Age
app-from-source  app-from-source.default.example.com  100% -> app-from-source  -            3 OK / 3    38m
helloworld-go    helloworld-go.default.example.com    100% -> helloworld-go    -            3 OK / 3    4d
simple-app       simple-app.default.example.com       100% -> simple-app       -            0 OK / 3    3h

3 routes

Succeeded

$ kubectl get service/app-from-source --namespace=default
NAME              TYPE           CLUSTER-IP   EXTERNAL-IP                                             PORT(S)   AGE
app-from-source   ExternalName   <none>       knative-ingressgateway.istio-system.svc.cluster.local   <none>    48m

$ kubectl get services.serving.knative.dev
NAME              AGE
app-from-source   53m
helloworld-go     4d
simple-app        4h
