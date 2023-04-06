Best Pratices in Monitoring a Kubernetes Cluster

## Install kube-prometheus-stack
### Create a monitoring namespace
+ `kubectl create namespace monitoring`

### Add prometheus-community repo
- `helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`
- `helm repo update prometheus-community`

### Use helm to install the kube-prometheus-stack
- `helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack -n monitoring`


## Poke around prom, alertmanager grafana and alertmanager
Prometheus and Alertmanager Web Panel

+ `kubectl port-forward svc/kube-prometheus-stack-prometheus 9090:9090 -n monitoring`

+ `kubectl port-forward alertmanager-kube-prometheus-stack-alertmanager-0 9093 -n monitoring`

Grafana Web Panel

+ `kubectl port-forward svc/kube-prometheus-stack-grafana 3000:80 -n monitoring`

## Deploy Grafana Loki (values.yaml -> go to the gist for infos)

+ `helm repo add grafana https://grafana.github.io/helm-charts`

+ `helm install loki --namespace=monitoring -f values.yaml grafana/loki-stack`


## Uninstall Helm chart

+ `helm delete kube-prometheus-stack -n monitoring`

&nbsp;

```kubectl delete crd alertmanagerconfigs.monitoring.coreos.com
kubectl delete crd alertmanagers.monitoring.coreos.com
kubectl delete crd podmonitors.monitoring.coreos.com
kubectl delete crd probes.monitoring.coreos.com
kubectl delete crd prometheuses.monitoring.coreos.com
kubectl delete crd prometheusrules.monitoring.coreos.com
kubectl delete crd servicemonitors.monitoring.coreos.com
kubectl delete crd thanosrulers.monitoring.coreos.com