# Experiments With Knative

Kuberenets Version:

```
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"9", GitVersion:"v1.9.0", GitCommit:"925c127ec6b946659ad0fd596fa959be43f0cc05", GitTreeState:"clean", BuildDate:"2017-12-15T21:07:38Z", GoVersion:"go1.9.2", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.11", GitCommit:"637c7e288581ee40ab4ca210618a89a555b6e7e9", GitTreeState:"clean", BuildDate:"2018-11-26T14:25:46Z", GoVersion:"go1.9.3", Compiler:"gc", Platform:"linux/amd64"}
```

Kubernetes Nodes:

```
$ kubectl get nodes
NAME                 STATUS    ROLES     AGE       VERSION
docker-for-desktop   Ready     master    9m        v1.10.11
```

Kubernetes Pods:

```
$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                         READY     STATUS    RESTARTS   AGE
docker        compose-74649b4db6-44fz2                     1/1       Running   0          6m
docker        compose-api-65975979ff-p4tqb                 1/1       Running   0          6m
kube-system   etcd-docker-for-desktop                      1/1       Running   0          7m
kube-system   kube-apiserver-docker-for-desktop            1/1       Running   0          6m
kube-system   kube-controller-manager-docker-for-desktop   1/1       Running   0          7m
kube-system   kube-dns-86f4d74b45-fg9mz                    3/3       Running   0          8m
kube-system   kube-proxy-j66t6                             1/1       Running   0          8m
kube-system   kube-scheduler-docker-for-desktop            1/1       Running   0          7m
```

Installing Istio:

```
$ curl -L https://github.com/knative/serving/releases/download/v0.2.2/istio.yaml \
| ->   | sed 's/LoadBalancer/NodePort/' \
| ->   | kubectl apply --filename -
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   601    0   601    0     0   1747      0 --:--:-- --:--:-- --:--:--  1752
  0     0    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--     0namespace "istio-system" created
configmap "istio-galley-configuration" created
configmap "istio-statsd-prom-bridge" created
configmap "istio-security-custom-resources" created
100  106k  100  106k    0     0  44524      0  0:00:02  0:00:02 --:--:-- 68984
configmap "istio" created
configmap "istio-sidecar-injector" created
serviceaccount "istio-galley-service-account" created
serviceaccount "istio-egressgateway-service-account" created
serviceaccount "istio-ingressgateway-service-account" created
serviceaccount "istio-mixer-service-account" created
serviceaccount "istio-pilot-service-account" created
serviceaccount "istio-cleanup-secrets-service-account" created
clusterrole "istio-cleanup-secrets-istio-system" created
clusterrolebinding "istio-cleanup-secrets-istio-system" created
job "istio-cleanup-secrets" created
serviceaccount "istio-citadel-service-account" created
serviceaccount "istio-sidecar-injector-service-account" created
customresourcedefinition "virtualservices.networking.istio.io" created
customresourcedefinition "destinationrules.networking.istio.io" created
customresourcedefinition "serviceentries.networking.istio.io" created
customresourcedefinition "gateways.networking.istio.io" created
customresourcedefinition "envoyfilters.networking.istio.io" created
customresourcedefinition "httpapispecbindings.config.istio.io" created
customresourcedefinition "httpapispecs.config.istio.io" created
customresourcedefinition "quotaspecbindings.config.istio.io" created
customresourcedefinition "quotaspecs.config.istio.io" created
customresourcedefinition "rules.config.istio.io" created
customresourcedefinition "attributemanifests.config.istio.io" created
customresourcedefinition "bypasses.config.istio.io" created
customresourcedefinition "circonuses.config.istio.io" created
customresourcedefinition "deniers.config.istio.io" created
customresourcedefinition "fluentds.config.istio.io" created
customresourcedefinition "kubernetesenvs.config.istio.io" created
customresourcedefinition "listcheckers.config.istio.io" created
customresourcedefinition "memquotas.config.istio.io" created
customresourcedefinition "noops.config.istio.io" created
customresourcedefinition "opas.config.istio.io" created
customresourcedefinition "prometheuses.config.istio.io" created
customresourcedefinition "rbacs.config.istio.io" created
customresourcedefinition "redisquotas.config.istio.io" created
customresourcedefinition "servicecontrols.config.istio.io" created
customresourcedefinition "signalfxs.config.istio.io" created
customresourcedefinition "solarwindses.config.istio.io" created
customresourcedefinition "stackdrivers.config.istio.io" created
customresourcedefinition "statsds.config.istio.io" created
customresourcedefinition "stdios.config.istio.io" created
customresourcedefinition "apikeys.config.istio.io" created
customresourcedefinition "authorizations.config.istio.io" created
customresourcedefinition "checknothings.config.istio.io" created
customresourcedefinition "kuberneteses.config.istio.io" created
customresourcedefinition "listentries.config.istio.io" created
customresourcedefinition "logentries.config.istio.io" created
customresourcedefinition "edges.config.istio.io" created
customresourcedefinition "metrics.config.istio.io" created
customresourcedefinition "quotas.config.istio.io" created
customresourcedefinition "reportnothings.config.istio.io" created
customresourcedefinition "servicecontrolreports.config.istio.io" created
customresourcedefinition "tracespans.config.istio.io" created
customresourcedefinition "rbacconfigs.rbac.istio.io" created
customresourcedefinition "serviceroles.rbac.istio.io" created
customresourcedefinition "servicerolebindings.rbac.istio.io" created
customresourcedefinition "adapters.config.istio.io" created
customresourcedefinition "instances.config.istio.io" created
customresourcedefinition "templates.config.istio.io" created
customresourcedefinition "handlers.config.istio.io" created
clusterrole "istio-galley-istio-system" created
clusterrole "istio-egressgateway-istio-system" created
clusterrole "istio-ingressgateway-istio-system" created
clusterrole "istio-mixer-istio-system" created
clusterrole "istio-pilot-istio-system" created
clusterrole "istio-citadel-istio-system" created
clusterrole "istio-sidecar-injector-istio-system" created
clusterrolebinding "istio-galley-admin-role-binding-istio-system" created
clusterrolebinding "istio-egressgateway-istio-system" created
clusterrolebinding "istio-ingressgateway-istio-system" created
clusterrolebinding "istio-mixer-admin-role-binding-istio-system" created
clusterrolebinding "istio-pilot-istio-system" created
clusterrolebinding "istio-citadel-istio-system" created
clusterrolebinding "istio-sidecar-injector-admin-role-binding-istio-system" created
service "istio-galley" created
service "istio-egressgateway" created
service "istio-ingressgateway" created
service "istio-policy" created
service "istio-telemetry" created
service "istio-statsd-prom-bridge" created
deployment "istio-statsd-prom-bridge" created
service "istio-pilot" created
service "istio-citadel" created
service "istio-sidecar-injector" created
deployment "istio-galley" created
deployment "istio-egressgateway" created
deployment "istio-ingressgateway" created
deployment "istio-policy" created
deployment "istio-telemetry" created
deployment "istio-pilot" created
deployment "istio-citadel" created
deployment "istio-sidecar-injector" created
gateway "istio-autogenerated-k8s-ingress" created
horizontalpodautoscaler "istio-egressgateway" created
horizontalpodautoscaler "istio-ingressgateway" created
horizontalpodautoscaler "istio-policy" created
horizontalpodautoscaler "istio-telemetry" created
horizontalpodautoscaler "istio-pilot" created
mutatingwebhookconfiguration "istio-sidecar-injector" created
attributemanifest "istioproxy" created
attributemanifest "kubernetes" created
stdio "handler" created
logentry "accesslog" created
logentry "tcpaccesslog" created
rule "stdio" created
rule "stdiotcp" created
metric "requestcount" created
metric "requestduration" created
metric "requestsize" created
metric "responsesize" created
metric "tcpbytesent" created
metric "tcpbytereceived" created
prometheus "handler" created
rule "promhttp" created
rule "promtcp" created
kubernetesenv "handler" created
rule "kubeattrgenrulerule" created
rule "tcpkubeattrgenrulerule" created
kubernetes "attributes" created
destinationrule "istio-policy" created
destinationrule "istio-telemetry" created
```

Default Namespace:

```
kubectl get namespace default -o yaml
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: 2019-01-17T23:32:06Z
  name: default
  resourceVersion: "4"
  selfLink: /api/v1/namespaces/default
  uid: 19f44753-1ab0-11e9-a8d0-025000000001
spec:
  finalizers:
  - kubernetes
status:
  phase: Active
  ```
  
Label the default namespace with istio-injection=enabled
  
```
$ kubectl label namespace default istio-injection=enabled
namespace "default" labeled
```
  
Default Namespace:

```
kubectl get namespace default -o yaml
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: 2019-01-17T23:32:06Z
  labels:
    istio-injection: enabled
  name: default
  resourceVersion: "1813"
  selfLink: /api/v1/namespaces/default
  uid: 19f44753-1ab0-11e9-a8d0-025000000001
spec:
  finalizers:
  - kubernetes
status:
  phase: Active
```

Pods under istio-system:

```
$ kubectl get pods --namespace istio-system
NAME                                        READY     STATUS    RESTARTS   AGE
istio-citadel-84fb7985bf-249lh              1/1       Running   0          4m
istio-egressgateway-bd9fb967d-cmlht         1/1       Running   0          4m
istio-galley-655c4f9ccd-r8gvw               1/1       Running   0          4m
istio-ingressgateway-688865c5f7-gqdhv       1/1       Running   0          4m
istio-pilot-6cd69dc444-md6xc                2/2       Running   0          4m
istio-policy-6b9f4697d-lzv4f                2/2       Running   0          4m
istio-sidecar-injector-8975849b4-qr77x      1/1       Running   0          4m
istio-statsd-prom-bridge-7f44bb5ddb-hs47f   1/1       Running   0          4m
istio-telemetry-6b5579595f-nqn4q            2/2       Running   0          4m
```

Installing Knative Serving

```
$ curl -L https://github.com/knative/serving/releases/download/v0.2.2/release-lite.yaml \| ->   | kubectl apply --filename -
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   608    0   608    0     0   1489      0 --:--:-- --:--:-- --:--:--  1493
  0  505k    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0namespace "knative-build" created
clusterrole "knative-build-admin" created
serviceaccount "build-controller" created
clusterrolebinding "build-controller-admin" created
customresourcedefinition "builds.build.knative.dev" created
customresourcedefinition "buildtemplates.build.knative.dev" created
customresourcedefinition "clusterbuildtemplates.build.knative.dev" created
customresourcedefinition "images.caching.internal.knative.dev" created
service "build-controller" created
service "build-webhook" created
image "creds-init" created
image "git-init" created
image "gcs-fetcher" created
image "nop" created
configmap "config-logging" created
deployment "build-controller" created
deployment "build-webhook" created
namespace "knative-serving" created
clusterrole "knative-serving-admin" created
clusterrole "knative-serving-write" created
serviceaccount "controller" created
serviceaccount "autoscaler" created
clusterrolebinding "knative-serving-controller-admin" created
clusterrolebinding "knative-serving-autoscaler-write" created
gateway "knative-shared-gateway" created
service "knative-ingressgateway" created
deployment "knative-ingressgateway" created
horizontalpodautoscaler "knative-ingressgateway" created
customresourcedefinition "clusteringresses.networking.internal.knative.dev" created
customresourcedefinition "configurations.serving.knative.dev" created
customresourcedefinition "images.caching.internal.knative.dev" configured
customresourcedefinition "podautoscalers.autoscaling.internal.knative.dev" created
customresourcedefinition "revisions.serving.knative.dev" created
customresourcedefinition "routes.serving.knative.dev" created
customresourcedefinition "services.serving.knative.dev" created
service "activator-service" created
service "controller" created
service "webhook" created
image "queue-proxy" created
image "fluentd-sidecar" created
deployment "activator" created
service "autoscaler" created
deployment "autoscaler" created
configmap "config-autoscaler" created
configmap "config-controller" created
configmap "config-domain" created
configmap "config-gc" created
configmap "config-logging" created
configmap "config-network" created
 19  505k   19  100k    0     0  26362      0  0:00:19  0:00:03  0:00:16 33462configmap "config-observability" created
deployment "controller" created
deployment "webhook" created
namespace "knative-monitoring" created
configmap "grafana-dashboard-definition-istio" created
configmap "grafana-dashboard-definition-mixer" created
configmap "grafana-dashboard-definition-pilot" created
serviceaccount "kube-state-metrics" created
role "kube-state-metrics-resizer" created
rolebinding "kube-state-metrics" created
clusterrole "kube-state-metrics" created
clusterrolebinding "kube-state-metrics" created
deployment "kube-state-metrics" created
service "kube-state-metrics" created
configmap "grafana-dashboard-definition-kubernetes-deployment" created
configmap "grafana-dashboard-definition-kubernetes-capacity-planning" created
 63  505k   63  321k    0     0  69385      0  0:00:07  0:00:04  0:00:03 84040configmap "grafana-dashboard-definition-kubernetes-cluster-health" created
configmap "grafana-dashboard-definition-kubernetes-cluster-status" created
configmap "grafana-dashboard-definition-kubernetes-control-plane-status" created
configmap "grafana-dashboard-definition-kubernetes-resource-requests" created
configmap "grafana-dashboard-definition-kubernetes-nodes" created
configmap "grafana-dashboard-definition-kubernetes-pods" created
configmap "grafana-dashboard-definition-kubernetes-statefulset" created
serviceaccount "node-exporter" created
clusterrole "node-exporter" created
clusterrolebinding "node-exporter" created
daemonset "node-exporter" created
service "node-exporter" created
configmap "grafana-dashboard-definition-knative-efficiency" created
100  505k  100  505k    0     0  95737      0  0:00:05  0:00:05 --:--:--  110k
configmap "scaling-config" created
configmap "grafana-dashboard-definition-knative" created
configmap "grafana-datasources" created
configmap "grafana-dashboards" created
service "grafana" created
deployment "grafana" created
metric "revisionrequestcount" created
metric "revisionrequestduration" created
metric "revisionrequestsize" created
metric "revisionresponsesize" created
prometheus "revisionpromhandler" created
rule "revisionpromhttp" created
configmap "prometheus-scrape-config" created
service "kube-controller-manager" created
service "prometheus-system-discovery" created
serviceaccount "prometheus-system" created
role "prometheus-system" created
role "prometheus-system" created
role "prometheus-system" created
role "prometheus-system" created
clusterrole "prometheus-system" created
rolebinding "prometheus-system" created
rolebinding "prometheus-system" created
rolebinding "prometheus-system" created
rolebinding "prometheus-system" created
clusterrolebinding "prometheus-system" created
service "prometheus-system-np" created
statefulset "prometheus-system" created
```

Pods under knative-serving:

```
$ kubectl get pods --namespace knative-serving
NAME                          READY     STATUS    RESTARTS   AGE
activator-df78cb6f9-dxktc     2/2       Running   0          48s
activator-df78cb6f9-fdhmd     2/2       Running   0          48s
activator-df78cb6f9-zhdnn     2/2       Running   0          48s
autoscaler-6fccb66768-ksjrk   2/2       Running   0          48s
controller-56cf5965f5-x8hvk   1/1       Running   0          47s
webhook-5dcbf967cd-nltpl      1/1       Running   0          47s

```

Create a new file named `service.yaml` with:

```
apiVersion: serving.knative.dev/v1alpha1 # Current version of Knative
kind: Service
metadata:
  name: helloworld-go # The name of the app
  namespace: default # The namespace the app will use
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: gcr.io/knative-samples/helloworld-go # The URL to the image of the app
            env:
              - name: TARGET # The environment variable printed out by the sample app
                value: "Go Sample v1"
```

Deploying app:

```
$ kubectl apply -f service.yaml
service "helloworld-go" created
```

Interacting with app:

```
$ INGRESSGATEWAY=knative-ingressgateway
$ kubectl get svc $INGRESSGATEWAY --namespace istio-system
NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                                                                                                                   AGE
knative-ingressgateway   LoadBalancer   10.107.234.251   <pending>     80:32380/TCP,443:32390/TCP,31400:32400/TCP,15011:31919/TCP,8060:30763/TCP,853:30198/TCP,15030:31652/TCP,15031:30108/TCP   7m
$ export IP_ADDRESS=$(kubectl get node  --output 'jsonpath={.items[0].status.addresses[0].address}'):$(kubectl get svc $INGRESSGATEWAY --namespace istio-system   --output 'jsonpath={.spec.ports[?(@.port==80)].nodePort}')
$ echo $IP_ADDRESS
192.168.65.3:32380
$ kubectl get routes/helloworld-go --output=custom-columns=NAME:.metadata.name,DOMAIN:.status.domain
NAME            DOMAIN
helloworld-go   helloworld-go.default.example.com
$ curl -H "Host: helloworld-go.default.example.com" http://${IP_ADDRESS}
```





References:

* https://github.com/knative/docs/blob/master/install/Knative-with-Docker-for-Mac.md
* https://github.com/knative/docs/blob/master/install/getting-started-knative-app.md


