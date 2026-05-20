# Multi-Container Pod Design Lab 🚀

A hands-on Kubernetes lab for learning:

- Init Containers
- Sidecar Containers
- Shared Volumes
- Logging Architecture
- Container Communication

---

## 📚 Table of Contents

- [Overview](#overview)
- [Objectives](#objectives)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Setup](#setup)
- [Task 1](#task-1)
- [Task 2](#task-2)
- [Troubleshooting](#troubleshooting)
- [Cleanup](#cleanup)

---

## 📖 Overview

This lab demonstrates how multiple containers work together inside a single Kubernetes Pod.

You will learn:

- Sidecar pattern
- Init container workflow
- Shared storage communication
- Logging and monitoring

---

## 🎯 Objectives

By completing this lab, you will:

- Create multi-container pods
- Configure init containers
- Implement sidecar logging
- Troubleshoot Kubernetes pods

---

## 🧱 Project Structure

```bash
lab2-multicontainer/
├── multi-container-pod.yaml
├── advanced-multi-container.yaml
└── README.md
```

---

## ⚙️ Setup

### Create Working Directory

```bash
mkdir ~/lab2-multicontainer
cd ~/lab2-multicontainer
```

### Verify Cluster

```bash
kubectl get nodes
```

---

# 🚀 Task 1

## Multi-Container Pod Architecture

| Container | Purpose |
|------------|----------|
| web-app | Runs Nginx |
| log-sidecar | Reads logs |
| config-setup | Initializes config |

---

## Create Manifest

```bash
nano multi-container-pod.yaml
```

---

## Deploy Pod

```bash
kubectl apply -f multi-container-pod.yaml
```

---

## Verify Pod

```bash
kubectl get pods
```

---

## Check Init Container Logs

```bash
kubectl logs multi-container-app -c config-setup
```

---

## Verify Configuration

```bash
kubectl exec multi-container-app -c web-app -- cat /etc/app-config/app.conf
```

---

# 🚀 Task 2

## Deploy Advanced Pod

```bash
kubectl apply -f advanced-multi-container.yaml
```

---

## View Logs

### Main Application

```bash
kubectl logs advanced-multi-container -c main-app
```

### Monitor Sidecar

```bash
kubectl logs advanced-multi-container -c monitor-sidecar
```

### Cleanup Sidecar

```bash
kubectl logs advanced-multi-container -c cleanup-sidecar
```

---

# 🛠 Troubleshooting

## Describe Pod

```bash
kubectl describe pod multi-container-app
```

---

## View Events

```bash
kubectl get events --field-selector involvedObject.name=multi-container-app
```

---

## Resource Usage

```bash
kubectl top pod multi-container-app --containers
```

---

# 🧹 Cleanup

```bash
kubectl delete pod multi-container-app
kubectl delete pod advanced-multi-container
```

---

# 📌 Key Concepts

## 🔹 Init Containers

Run before application containers start.

---

## 🔹 Sidecar Containers

Provide helper functionality like:

- Logging
- Monitoring
- Metrics

---

## 🔹 Shared Volumes

Allow containers in the same pod to share data.

---

# 🏆 CKAD Relevance

Topics covered:

- Multi-container Pods
- Init Containers
- Shared Volumes
- Troubleshooting

---

# 👨‍💻 Author

Zohaib Ahmed

DevOps | Kubernetes | Cloud | AI/ML

---

# ⭐ Support

Give this repository a star if it helped you.
