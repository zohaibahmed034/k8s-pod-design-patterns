# Multi-Container Pod Design Lab 🚀

A hands-on Kubernetes lab focused on understanding and implementing Multi-Container Pods using:

- Init Containers
- Sidecar Containers
- Shared Volumes
- Container Communication
- Logging Architecture
- Monitoring & Troubleshooting

This project is ideal for:

- Kubernetes Beginners
- DevOps Engineers
- CKAD Preparation
- Cloud-Native Developers

---

# 📚 Table of Contents

- [Overview](#-overview)
- [Objectives](#-objectives)
- [Prerequisites](#-prerequisites)
- [Project Structure](#-project-structure)
- [Environment Setup](#️-environment-setup)
- [Task 1 - Multi-Container Pod](#-task-1---multi-container-pod)
- [Task 2 - Advanced Multi-Container Pod](#-task-2---advanced-multi-container-pod)
- [Troubleshooting](#-troubleshooting)
- [Best Practices](#-best-practices)
- [Key Concepts](#-key-concepts)
- [CKAD Relevance](#-ckad-relevance)
- [Cleanup](#-cleanup)
- [Contributing](#-contributing)
- [Author](#-author)

---

# 📖 Overview

This lab demonstrates how multiple containers can work together inside a single Kubernetes Pod.

You will learn:

- Init container workflow
- Sidecar container pattern
- Shared storage communication
- Multi-container debugging
- Logging and monitoring patterns

---

# 🎯 Objectives

By completing this lab, you will be able to:

- Create multi-container pods
- Configure init containers
- Implement sidecar logging
- Share data between containers
- Troubleshoot pod issues
- Monitor container interactions
- Apply Kubernetes design patterns

---

# 🛠 Prerequisites

Before starting this lab, ensure you have:

- Basic Kubernetes knowledge
- Understanding of Pods & Containers
- YAML fundamentals
- Linux command-line basics
- kubectl configured
- Minikube or Kubernetes cluster

---

# 🧱 Project Structure

```bash
lab2-multicontainer/
│
├── multi-container-pod.yaml
├── advanced-multi-container.yaml
└── README.md
⚙️ Environment Setup
Create Working Directory
mkdir ~/lab2-multicontainer
cd ~/lab2-multicontainer
Verify Cluster
kubectl get nodes
🚀 Task 1 - Multi-Container Pod
Architecture
Container	Purpose
web-app	Runs Nginx application
log-sidecar	Monitors nginx logs
config-setup	Initializes configuration
Create Pod Manifest

Create YAML file:

nano multi-container-pod.yaml

Apply configuration:

kubectl apply -f multi-container-pod.yaml

Verify pod:

kubectl get pods
Verify Init Container

Check logs:

kubectl logs multi-container-app -c config-setup

Verify configuration file:

kubectl exec multi-container-app -c web-app -- cat /etc/app-config/app.conf
Generate Traffic

Get pod IP:

kubectl get pod multi-container-app -o wide

Run test client:

kubectl run test-client --image=busybox:1.35 --rm -it --restart=Never -- /bin/sh

Inside test pod:

wget -q -O- http://POD_IP
wget -q -O- http://POD_IP/nonexistent
Monitor Sidecar Logs
kubectl logs multi-container-app -c log-sidecar --tail=20
🚀 Task 2 - Advanced Multi-Container Pod
Create Advanced Pod
nano advanced-multi-container.yaml

Deploy:

kubectl apply -f advanced-multi-container.yaml
Advanced Architecture
Container	Role
data-initializer	Initializes shared data
main-app	Processes data
monitor-sidecar	Monitors files
cleanup-sidecar	Cleans temporary files
Monitor Containers
Main App Logs
kubectl logs advanced-multi-container -c main-app
Monitor Sidecar Logs
kubectl logs advanced-multi-container -c monitor-sidecar
Cleanup Sidecar Logs
kubectl logs advanced-multi-container -c cleanup-sidecar
🛠 Troubleshooting
Describe Pod
kubectl describe pod multi-container-app
View Events
kubectl get events --field-selector involvedObject.name=multi-container-app
Resource Usage
kubectl top pod multi-container-app --containers
Access Container Shell
kubectl exec -it multi-container-app -c web-app -- /bin/sh
📦 Shared Volume Verification

Write file from web container:

kubectl exec multi-container-app -c web-app -- sh -c 'echo "Hello" > /var/log/nginx/test.txt'

Read from sidecar:

kubectl exec multi-container-app -c log-sidecar -- cat /var/log/nginx/test.txt
📌 Key Concepts
🔹 Init Containers

Used for:

Configuration setup
Dependency checks
Secret initialization
🔹 Sidecar Containers

Used for:

Logging
Monitoring
Proxying
Metrics collection
🔹 Shared Volumes

Enable containers inside the same pod to share data.

Example:

volumes:
- name: shared-data
  emptyDir: {}
📖 Best Practices
Use one responsibility per container
Use init containers for setup tasks
Use sidecars for auxiliary services
Monitor resource usage
Use meaningful naming conventions
Store YAML manifests in version control
🏆 CKAD Relevance

This lab covers important CKAD topics:

Multi-container Pods
Init Containers
Shared Volumes
Logging
Troubleshooting
🧹 Cleanup

Delete resources:

kubectl delete pod multi-container-app
kubectl delete pod advanced-multi-container

Verify cleanup:

kubectl get pods
🤝 Contributing

Contributions are welcome.

You can improve this project by adding:

Helm charts
Deployments
ConfigMaps
Persistent Volumes
Monitoring integrations
👨‍💻 Author
Zohaib Ahmed

DevOps Engineer | Kubernetes | Cloud | AI/ML Enthusiast

⭐ Support

If you found this repository useful, give it a star ⭐

📜 License

This project is licensed under the MIT License.
