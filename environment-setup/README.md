# Production Environment Setup

Preparation of the environment to ensure proper running of deployed applications.

## Ingress Controller

Install Kubernetes Nginx Ingress Controller

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml`

Creates ingress-nginx namespaces and deploys resources into the namespace

`kubectl get pods --namespace=ingress-nginx`

## Storage

Create Storage class with `local` provisioner ([Yaml File](./sc.yaml))

