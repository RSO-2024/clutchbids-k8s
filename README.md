


# Helm chart

Create chart:
`helm create gateway-chart`

Install chart:
`helm install k8s-clutchbids-chart ./k8s-clutchbids-chart --kubeconfig=k8s-clutchbids-kubeconfig.yaml`

Upgrade chart:
`helm upgrade k8s-clutchbids-chart ./k8s-clutchbids-chart --kubeconfig=k8s-clutchbids-kubeconfig.yaml`

## Clutchbids Helm charts

- `p2p-listings-chart`

# Monitoring

Check if monitoring stack is running:
`helm ls -n kube-prometheus-stack --kubeconfig=k8s-clutchbids-kubeconfig.yaml`

Accessing Prometheus Web Panel:
`kubectl --kubeconfig=k8s-clutchbids-kubeconfig.yaml port-forward svc/kube-prometheus-stack-prometheus 9090:9090 -n kube-prometheus-stack`

Accessing Grafana Web Panel:
`kubectl --kubeconfig=k8s-clutchbids-kubeconfig.yaml port-forward svc/kube-prometheus-stack-grafana 3000:80 -n kube-prometheus-stack`

Adding additional service monitors (edit `kube-prometheus-values.yaml`):
`helm upgrade kube-prometheus-stack prometheus-community/kube-prometheus-stack --kubeconfig=k8s-clutchbids-kubeconfig.yaml --namespace kube-prometheus-stack --values kube-prometheus-values.yaml`

# Gateway API

- [Tutorial link](https://semaphoreci.com/blog/kubernetes-gateway-api)

Check status:
`kubectl --kubeconfig=k8s-clutchbids-kubeconfig.yaml -n envoy-gateway-system get po,svc,deploy`