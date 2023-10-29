# Helm

```bash
$ helm create hello
$ helm install hello hello
$ helm status hello
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
