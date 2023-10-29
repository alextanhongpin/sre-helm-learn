# Helm

```bash
$ helm create hello
$ helm install hello hello
$ helm status hello
$ helm test hello
```

## Debug

https://kubernetes.io/docs/tutorials/services/connect-applications-service/

Debug using ephemeral container:


```bash
$ k debug -it hello-675977564-5cp5b  --image=busybox:1.28 --target hello
Targeting container "hello". If you don't see processes from this container it may be because the container runtime doesn't support this feature.
Defaulting debug container name to debugger-z4c6l.
If you don't see a command prompt, try pressing enter.
/ #
```

Running another container to test pod to pod communication:

```bash
$ k run -it --rm --image=busybox:1.28 -- sh
$ k delete po sh
```

To check the environment variable:

```bash
/ # printenv  | grep SERVICE
KUBERNETES_SERVICE_PORT=443
HELLO_SERVICE_PORT_HTTP=80
HELLO_SERVICE_HOST=10.96.223.228
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_SERVICE_HOST=10.96.0.1
HELLO_SERVICE_PORT=80
```

Alternative if you want to run curl/nslookup:

```bash
$ kubectl run curl --image=radial/busyboxplus:curl -i --tty --rm
```

## Running Test


```bash
$ helm test hello
```


Output:

```
NAME: hello
LAST DEPLOYED: Mon Oct 30 00:33:10 2023
NAMESPACE: default
STATUS: deployed
REVISION: 14
TEST SUITE:     hello-test-connection
Last Started:   Mon Oct 30 00:33:12 2023
Last Completed: Mon Oct 30 00:33:20 2023
Phase:          Succeeded
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=hello,app.kubernetes.io/instance=hello" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
```
