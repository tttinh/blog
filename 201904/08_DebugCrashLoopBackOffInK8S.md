# Troubleshoot CrashLoopBackOff Event In K8S Cluster

[Home](../README.md)

## What is a CrashLoopBackOff?

A `CrashloopBackOff` means that you have a pod starting, crashing, starting again, and then crashing again.

A `PodSpec` has a `restartPolicy` field with possible values `Always`, `OnFailure`, and `Never` which applies to all containers in a pod. The default value is `Always` and the `restartPolicy` only refers to restarts of the containers by the `kubelet` on the same node (so the restart count will reset if the pod is rescheduled in a different node). Failed containers that are restarted by the `kubelet` are restarted with an exponential back-off delay (10s, 20s, 40s …) capped at five minutes, and is reset after ten minutes of successful execution. This is an example of a `PodSpec` with the `restartPolicy` field:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dummy-pod
spec:
  containers:
    - name: dummy-pod
      image: ubuntu
  restartPolicy: Always
```

## Why does a CrashLoopBackOff occur?

1.  The application inside the container keeps crashing.
2.  Some type of parameters of the pod or container have been configured incorrectly.
3.  An error has been made when deploying Kubernetes.

## How can I see if there are CrashLoopBackOff in my cluster?

Run `kubectl get pods` command and we’ll be able to see the status of any pod that is currently in `CrashLoopBackOff`:

```bash
kubectl get pods --namespace nginx-crashloop
NAME                     READY     STATUS             RESTARTS   AGE
flask-7996469c47-d7zl2   1/1       Running            1          77d
flask-7996469c47-tdr2n   1/1       Running            0          77d
nginx-5796d5bc7c-2jdr5   0/1       CrashLoopBackOff   2          1m
nginx-5796d5bc7c-xsl6p   0/1       CrashLoopBackOff   2          1m
```

Actually if we see pods in `Error` status, probably they will get into `CrashLoopBackOff` soon:

```bash
kubectl get pods --namespace nginx-crashloop
NAME                     READY     STATUS    RESTARTS   AGE
flask-7996469c47-d7zl2   1/1       Running   1          77d
flask-7996469c47-tdr2n   1/1       Running   0          77d
nginx-5796d5bc7c-2jdr5   0/1       Error     0          24s
nginx-5796d5bc7c-xsl6p   0/1       Error     0          24s
```

## How to troubleshoot a CrashLoopBackOff?

Often we'll easly spot the issue after run:

```bash
kubectl describe [-o yaml] <nodes, pods, svc...>
```

If not, let's investigate the summarized view of recently-seen events, using:

```bash
kubectl get events
```

Then let's see the logs from a container in a pod. If the pod has multiple
containers, container must be specified with -c.

```bash
kubectl logs
```
