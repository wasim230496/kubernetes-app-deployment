# DevOps Project 3 - Kubernetes Orchestration Notes

## Environment

* AWS EC2 (Ubuntu 22.04)
* Docker
* Kind (Kubernetes in Docker)
* kubectl
* GitHub

---

# Installation Commands

## Update Server

```bash
sudo apt update && sudo apt upgrade -y
```

## Install Docker

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

Verify:

```bash
docker --version
docker ps
```

Add current user to Docker group:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

Verify:

```bash
docker ps
```

---

## Install Kind

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64

chmod +x kind

sudo mv kind /usr/local/bin/
```

Verify:

```bash
kind version
```

---

## Install kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl

sudo mv kubectl /usr/local/bin/
```

Verify:

```bash
kubectl version --client
```

---

# Cluster Commands

## Create Cluster

```bash
kind create cluster --name devops-project3
```

## Check Cluster

```bash
kubectl cluster-info
```

```bash
kubectl get nodes
```

## Delete Cluster

```bash
kind delete cluster --name devops-project3
```

---

# Deployment Commands

Apply Deployment

```bash
kubectl apply -f deployment.yaml
```

View Deployments

```bash
kubectl get deployment
```

View Pods

```bash
kubectl get pods
```

Detailed Information

```bash
kubectl describe pod <pod-name>
```

Delete Deployment

```bash
kubectl delete -f deployment.yaml
```

---

# Service Commands

Create Service

```bash
kubectl apply -f service.yaml
```

Check Services

```bash
kubectl get svc
```

Describe Service

```bash
kubectl describe svc nginx-service
```

---

# Scaling Commands

Scale to 5 Pods

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Check Pods

```bash
kubectl get pods
```

Scale Back to 2 Pods

```bash
kubectl scale deployment nginx-deployment --replicas=2
```

---

# Ingress Commands

Install Ingress Controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

Check Ingress Pods

```bash
kubectl get pods -n ingress-nginx
```

Create Ingress

```bash
kubectl apply -f ingress.yaml
```

Check Ingress

```bash
kubectl get ingress
```

---

# Useful Debugging Commands

View Everything

```bash
kubectl get all
```

View Logs

```bash
kubectl logs <pod-name>
```

Open Shell Inside Pod

```bash
kubectl exec -it <pod-name> -- /bin/bash
```

Describe Pod

```bash
kubectl describe pod <pod-name>
```

Check Events

```bash
kubectl get events
```

Check Node Status

```bash
kubectl get nodes
```

---

# Common Errors & Fixes

## Error: Docker Permission Denied

```bash
permission denied while trying to connect to Docker daemon
```

Fix:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

## Error: kubectl Cannot Connect

```bash
The connection to the server localhost:8080 was refused
```

Fix:

Check cluster status:

```bash
kind get clusters
```

If missing:

```bash
kind create cluster --name devops-project3
```

---

## Error: Pod CrashLoopBackOff

Check logs:

```bash
kubectl logs <pod-name>
```

Check details:

```bash
kubectl describe pod <pod-name>
```

Usually caused by:

* Wrong image
* Application startup failure
* Incorrect environment variables

---

## Error: ImagePullBackOff

Causes:

* Wrong image name
* Private image
* Typo in repository

Check:

```bash
kubectl describe pod <pod-name>
```

---

## Error: Service Not Working

Check labels:

```bash
kubectl get pods --show-labels
```

Verify selector matches Deployment labels.

---

## Error: Ingress Not Accessible

Check:

```bash
kubectl get pods -n ingress-nginx
```

Ingress controller must be Running.

---

# Lessons Learned

1. Kubernetes manages containers through Pods.
2. Deployments provide self-healing and scaling.
3. Services provide stable networking and load balancing.
4. Ingress exposes applications externally.
5. YAML files are declarative infrastructure definitions.
6. Kubernetes automatically recreates failed Pods.
7. Labels and selectors are critical for Service communication.
8. kubectl describe and kubectl logs are the most useful troubleshooting commands.
9. Scaling applications requires only changing replica counts.
10. Infrastructure as Code makes environments reproducible.

---

# Interview Questions from This Project

Q: What is a Pod?
A: Smallest deployable unit in Kubernetes that runs one or more containers.

Q: What is a Deployment?
A: A controller that manages Pods and provides scaling and self-healing.

Q: What is a Service?
A: Provides stable networking and load balancing for Pods.

Q: What is Ingress?
A: Routes external HTTP/HTTPS traffic into Kubernetes services.

Q: Difference between Deployment and Pod?
A: Pods are containers; Deployments manage Pods.

Q: How do you scale an application?
A: Using replicas in Deployment or kubectl scale command.

Q: How do you troubleshoot a failing Pod?
A: kubectl logs and kubectl describe pod.

Q: What happens if a Pod crashes?
A: Kubernetes automatically creates a new Pod.
