
# Blue-Green Deployment using Docker and Kubernetes

## Project Overview

This project demonstrates Blue-Green Deployment using Docker and Kubernetes.

Two separate application environments are maintained:

- **Blue Environment:** Version 1.0
- **Green Environment:** Version 2.0

Both environments run independently using Kubernetes Deployments. Kubernetes Service is used to route traffic between Blue and Green environments by changing its selector.

## Technologies Used

- Docker
- Kubernetes
- Minikube
- Jenkins
- Git
- GitHub
- Nginx

## Project Architecture

1. Create separate Blue and Green application versions.
2. Build Docker images for both versions.
3. Deploy both applications on Kubernetes.
4. Create a Kubernetes NodePort Service.
5. Route traffic to Blue using the Service selector.
6. Switch traffic to Green by updating the Service selector.

## Project Structure

```text
Blue-Green-Deployment/
│
├── app/
│   ├── blue/
│   │   └── index.html
│   ├── green/
│   │   └── index.html
│   ├── Dockerfile
│   └── index.html
│
├── k8s/
│   ├── blue-deployment.yaml
│   ├── green-deployment.yaml
│   └── service.yaml
│
├── screenshots/
├── Jenkinsfile
└── README.md
```

## Docker Image Build

Build Blue image:

```bash
docker build -t blue-app:v1 -f app/Dockerfile app/blue
```

Build Green image:

```bash
docker build -t green-app:v2 -f app/Dockerfile app/green
```

## Kubernetes Deployment

Start Minikube:

```bash
minikube start
```

Create namespace:

```bash
kubectl create namespace blue-green
```

Load Docker images into Minikube:

```bash
minikube image load blue-app:v1
minikube image load green-app:v2
```

Deploy Blue application:

```bash
kubectl apply -f k8s/blue-deployment.yaml
```

Deploy Green application:

```bash
kubectl apply -f k8s/green-deployment.yaml
```

Create Kubernetes Service:

```bash
kubectl apply -f k8s/service.yaml
```

Check Pods:

```bash
kubectl get pods -n blue-green
```

## Access Application

Run:

```bash
minikube service blue-green-service -n blue-green --url
```

Open the generated URL in your browser.

The initial Service selector points to the Blue environment.

## Switching Traffic from Blue to Green

Initially, the Service selector is:

```yaml
selector:
  app: blue
```

To route traffic to Green, update it to:

```yaml
selector:
  app: green
```

Apply the updated Service:

```bash
kubectl apply -f k8s/service.yaml
```

Refresh the same browser URL to view the Green application.

## Verify Service

```bash
kubectl describe svc blue-green-service -n blue-green
```

Check the Service selector and endpoints to verify the active environment.

## Jenkins Integration

A Jenkins Pipeline job has been created with an initial Docker availability check.

Further CI/CD automation can be added to build images, publish them to Docker Hub, and update Kubernetes Deployments.

## Key Learning Outcomes

- Building Docker images for separate application versions.
- Deploying multiple application environments using Kubernetes.
- Managing Kubernetes Deployments and Services.
- Understanding Service selectors and Pod endpoints.
- Switching application traffic between Blue and Green environments.
- Using Git and GitHub for source code management.
