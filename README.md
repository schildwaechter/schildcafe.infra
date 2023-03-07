# SchildCafé: Infrastructure

![SchildCafé](logo.png)


This is the [Kubernetes](https://kubernetes.io/) setup for the SchildCafé.

## Use

Tested with [Minikube](https://minikube.sigs.k8s.io/docs/).

```
minikube config set memory 3819
minikube config set driver virtualbox
minikube start
minikube addons enable metrics-server
minikube dashboard &
minikube tunnel
```

### SchildCafé

The SchildCafé is installed from the `k8s` folder with [`kubectl`](https://kubernetes.io/docs/reference/kubectl/kubectl/).

```
kubectl apply -f k8s/ns.yaml
kubectl apply -f k8s/mysql-secret.yaml -f k8s/mysql-storage.yaml -f k8s/mysql-deployment.yaml
kubectl apply -f k8s/coffee-deployment.yaml -f k8s/servitor-deployment.yaml -f k8s/barista-cronjob.yaml
```

Get the Servitør API endpoint
```
minikube service schildcafe-servitor -n schildcafe --url
```


Set up monitoring (requires [`helm`](https://helm.sh/))
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts

kubectl create namespace obs
helm install prometheus -n obs prometheus-community/prometheus
helm install loki -n obs grafana/loki-stack
helm install grafana -n obs grafana/grafana -f obs/grafana.yaml
kubectl apply -n obs -f obs/configmap-grafana.yaml
kubectl rollout restart deployment/grafana -n obs
helm install adapter -n obs prometheus-community/prometheus-adapter -f obs/prometheus-adapter.yaml
```

Check for the presence of the custom metric
```
kubectl get --raw "/apis/custom.metrics.k8s.io/v1beta1/namespace/*/job_queue_length" | jq
```

Then configure the Horizontal Pod Autoscaler for the coffee machine
```
kubectl apply -f k8s/coffee-scaler.yaml
```
and watch
```
kubectl get hpa -n schildcafe --watch
```



## Namespace

The namespace is `schildcafe`.

```
kubectl config set-context --current --namespace=schildcafe
```
