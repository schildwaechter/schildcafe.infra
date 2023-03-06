# SchildCafé: Infrastructure

![SchildCafé](logo.png)


This is the [Kubernetes](https://kubernetes.io/) setup for the SchildCafé.

## Use

Tested with [Minikube](https://minikube.sigs.k8s.io/docs/).

```
minikube config set memory 3819
minikube start
minikube addons enable metrics-server
minikube dashboard &
minikube tunnel
```

### SchildCafé

The SchildCafé is installed from the `k8s` folder with `kubectl`.

Use
```
kubectl apply -f ns.yaml
kubectl apply -f mysql-secret.yaml -f mysql-storage.yaml -f mysql-deployment.yaml
kubectl apply -f coffee-deployment.yaml -f servitor-deployment.yaml -f barista-cronjob.yaml
```
in the `k8s` directory to get all components up.

Then run
```
minikube service schildcafe-servitor -n schildcafe --url
```
to get the API endpoint.

Make sure to install the monitoring.

Then configure the Horizontal Pod Autoscaler for the coffee machine
```
kubectl apply -f coffee-scaler.yaml
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
