## Kubernetes Runbooks

You should be [familiar with Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/) (k8s). We use lots of `Service`, `Deployment`, `Ingress` and `PersistentVolumeClaim` objects along with a few others where needed. Our clusters run with [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) on Google's Kubernetes Engine (GKE).

**Links**: [infra-oss.moov.io](https://infra-oss.moov.io) | [Google Cloud Status](https://status.cloud.google.com/) | [GKE Dashboard](https://console.cloud.google.com/kubernetes/list)

There are also several community guides for troubleshooting Kubernetes problems:

- [Kubernetes.io Guide](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-cluster/)
- [Cloud.gov Guide](https://cloud.gov/docs/ops/runbook/troubleshooting-kubernetes/)
- [Codefresh.io Guide](https://codefresh.io/Kubernetes-Tutorial/recover-broken-kubernetes-cluster/)
- [Kubernetes Basics](https://www.youtube.com/playlist?list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT) (Videos)

**Useful Tools**

- `kubespy`: Tool for observing Kubernetes resources in real time - [GitHub](https://github.com/pulumi/kubespy)
   - [kubespy trace: a real-time view into the heart of a Kubernetes Service](https://blog.pulumi.com/kubespy-trace-a-real-time-view-into-the-heart-of-a-kubernetes-service)

### Viewing Pod/Container logs

```
$ kubectl get pods -n infra  | grep kube-ingress
kube-ingress-index-5cb86955ff-md64n   1/1       Running   0          18m
kube-ingress-index-5cb86955ff-xdb5m   1/1       Running   0          18m

# --tail only shows the last N logs
# -f keeps tailing the pod/container stdout
$ kubectl logs -n infra [--tail 10] [-f] kube-ingress-index-5cb86955ff-xdb5m
...
```

See also: [Viewing logs in Kubernetes](https://medium.com/devopslinks/viewing-logs-in-kubernetes-e055f936e187)

### Viewing Logs with Loki / Grafana

[Loki](https://github.com/grafana/loki) is a new log aggregation platform which attempts to transform logs into metric streams (with log information as labels). This project is new, but Grafana allows exploring, building dashboards, and alerts. Checkout the [explore page showing paygate logs](https://infra-oss.moov.io/grafana/explore?left=%5B"now%2Fd","now%2Fd","Loki",%7B"expr":"%7Bapp%3D%5C"paygate%5C"%7D"%7D,%7B"ui":%5Btrue,true,true,"none"%5D%7D%5D) and the [basic usage guide](https://github.com/grafana/loki/blob/master/docs/usage.md#searching-with-labels-and-distributed-grep).

- [Loki Troubleshooting Guide](https://github.com/grafana/loki/blob/master/docs/troubleshooting.md)
- [An early preview of Loki](https://itnext.io/grafana-logging-using-loki-45665916aec9)
- [Loki Metrics](https://github.com/grafana/loki/blob/master/docs/operations/observability.md#observing-loki)

### Rolling Pods / Containers

If you need to restart a Pod/Container simply list out the pods and issue `kubectl delete`:

```
$ kubectl get pods -n infra  | grep kube-ingress
kube-ingress-index-5cb86955ff-md64n   1/1       Running   0          18m
kube-ingress-index-5cb86955ff-xdb5m   1/1       Running   0          18m

$ kubectl delete pod -n infra kube-ingress-index-5cb86955ff-rtdms
pod "kube-ingress-index-5cb86955ff-rtdms" deleted
```

### Node Sizing / Availability

Currently our Kubernetes cluster runs on preemptible instances which can terminate themselves in under 60s. We largely do this for cost savings before having a product, but will likely run a combination of permanent and preemptible nodes going forward. It's important to remember several guidelines: ([Source](https://learnk8s.io/blog/kubernetes-spot-instances))

- Have a backup plan (permanent node pool)
- Find unpopular instance sizes
   - If a new family comes out (i.e. m5) m4's might become cheaper and less requested.
- Set a maximum bid price
- Run multi-zone setups to avoid shortages in a single GCP zone

### Emacs

[chrisbarrett/kubernetes-el](https://github.com/chrisbarrett/kubernetes-el) works with our setup. Talk to @adamdecaf for help.
