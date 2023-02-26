# SchildCafé: Infrastructure

This is the Kubernetes setup for the SchildCafé.

## Use

Tested with minikube.

Run
```
minikube service schildcafe-servitor -n schildcafe --url
```
to get the API endpoint.

## Namespace

The namespace is `schildcafe`.

```
kubectl config set-context --current --namespace=schildcafe
```

## Minikube

```
minikube service schildcafe-servitor -n schildcafe --url
```
