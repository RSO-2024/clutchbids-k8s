
# Docker compose

Create a docker compose file and then do the following:

Docker build:
`docker-compose build`

Docker push registry:
`docker-compose push`

> Make sure to have Digital Ocean registry set in docker compose file.

# Secrets & ConfigMap 

Building secrets from `.env` file:
`kubectl --kubeconfig=k8s-clutchbids-kubeconfig.yaml create secret generic [secret_name] --from-env-file=.env`

Editing ConfigMap dynamically:
`kubectl --kubeconfig=k8s-clutchbids-kubeconfig.yaml edit configmap [config_name]`
> Then afterwards you must call the `/config/reload` endpoint to reload the configuration that was dynamically changed.

# Helm chart

Create chart:
`helm create [chart_name]`

Install chart:
`helm install [chart_name] ./[chart_name] --kubeconfig=k8s-clutchbids-kubeconfig.yaml`

Upgrade chart:
`helm upgrade [chart_name] ./[chart_name] --kubeconfig=k8s-clutchbids-kubeconfig.yaml`

Changing between Helm chart values (prod/dev):
`helm upgrade [chart_name]  ./[chart_name] --kubeconfig=k8s-clutchbids-kubeconfig.yaml --values ./[chart_name]/values-[prod/dev].yaml`

## Clutchbids Helm charts

Each of the following microservices:
- `p2p-listings-chart`
- `p2p-price-auctions-chart`
- `p2p-flash-auctions-chart`

contains a Service, ConfigMap (to use additional non-sensitive specific configurations) and Deployment with Helm charts to deploy to Kubernetes cluster.

- `clutchbids-gw-routes-chart` - used for HTTPRoutes, deployed with Helm

# Monitoring

Check if monitoring stack is running:
`helm ls -n kube-prometheus-stack --kubeconfig=k8s-clutchbids-kubeconfig.yaml`

Accessing Prometheus Web Panel:
`kubectl --kubeconfig=k8s-clutchbids-kubeconfig.yaml port-forward svc/kube-prometheus-stack-prometheus 9090:9090 -n kube-prometheus-stack`

Accessing Grafana Web Panel:
`kubectl --kubeconfig=k8s-clutchbids-kubeconfig.yaml port-forward svc/kube-prometheus-stack-grafana 3000:80 -n kube-prometheus-stack`

Adding additional service monitors (edit `kube-prometheus-values.yaml`) - [tutorial link](https://marketplace.digitalocean.com/apps/kubernetes-monitoring-stack):
`helm upgrade kube-prometheus-stack prometheus-community/kube-prometheus-stack --kubeconfig=k8s-clutchbids-kubeconfig.yaml --namespace kube-prometheus-stack --values kube-prometheus-values.yaml`

# Gateway API

- [Tutorial link](https://semaphoreci.com/blog/kubernetes-gateway-api)

Check status:
`kubectl --kubeconfig=k8s-clutchbids-kubeconfig.yaml -n envoy-gateway-system get po,svc,deploy`

Deploy gateway route changes with Helm:
`helm upgrade clutchbids-gw-routes-chart ./clutchbids-gw-routes-chart  --kubeconfig=k8s-clutchbids-kubeconfig.yaml`