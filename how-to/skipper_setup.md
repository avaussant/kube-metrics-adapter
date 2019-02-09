# Skipper Prometheus Metrics Collection

The skipper-ingress pods should be configured to be scraped by Prometheus. This
can be done by Prometheus service discovery using discovery of Kubernetes services
or Kubernetes pods:

```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    prometheus.io/path: /metrics
    prometheus.io/port: "9911"
    prometheus.io/scrape: "true"
  labels:
    application: skipper-ingress
  name: skipper-ingress
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 9999
  selector:
    application: skipper-ingress
  type: ClusterIP
```

The annotations `prometheus.io/path`, `prometheus.io/port` and `prometheus.io/scrape`
instruct Prometheus to scrape all pods of this service on the port _9911_ and
the path `/metrics`.

When the `kube-metrics-adapter` is started the following flag should be set so that
the adapter can query prometheus to get aggregated metrics.