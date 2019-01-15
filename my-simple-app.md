 kubectl create ns my-simple-app

 export KNCTL_NAMESPACE=my-simple-app

 knctl service list
   Services in namespace 'my-simple-app'

   Name  Domain  Annotations  Conditions  Age

   0 services

   Succeeded 

 kubectl get pods -n $KNCTL_NAMESPACE
   No resources found.

 Configure Knative with the container registry location and secrets using knctl basic-auth-secret create within each applicable Kubernetes namespace.
 knctl basic-auth-secret create -s registry --docker-hub -u <username> -p <password>

 Map the container registry secrets into a Kubernetes service account, which will provide the information above into the pods used by Knative Build.
 knctl service-account create --service-account build -s registry
   Name  build

   Succeeded 

 Verify mapping down to a Kubernetes Service Account: 
 kubectl get serviceaccount -n $KNCTL_NAMESPACE
   NAME      SECRETS   AGE
   build     2         50s
   default   1         10m

 Installing kail - Kubernetes Tail
 brew install boz/repo/kail

 Register build template with the name "buildpack" into your active namespace 
 kubectl -n $KNCTL_NAMESPACE apply -f https://raw.githubusercontent.com/knative/build-templates/master/buildpack/buildpack.yaml
   buildtemplate "buildpack" created

 list of the buildtemplate just registered
 kubectl get buildtemplate -n $KNCTL_NAMESPACE
   NAME        AGE
   buildpack   21s

 At this point, its ready to authorize Knative Build subsystem to create new container images.

 git clone https://github.com/cppforlife/simple-app

 cd simple-app

 DOCKER_IMAGE=index.docker.io/<docker username>/knative-simple-app

 knctl deploy \
    --service simple-app \
    --directory=$PWD \
    --service-account build \
    --image ${DOCKER_IMAGE:?required} \
    --env SIMPLE_MSG="Built from local directory using Buildpack template" \
    --template buildpack \
    --template-env GOPACKAGENAME=main


 OR
 knctl deploy \
    --service simple-app \
    --git-url https://github.com/cppforlife/simple-app.git \
    --git-revision master \
    --service-account build \
    --image ${DOCKER_IMAGE:?required} \
    --env SIMPLE_MSG="Built from Git repo using Buildpack template" \
    --template buildpack \
    --template-env GOPACKAGENAME=main

 knctl curl -s simple-app -n $KNCTL_NAMESPACE

