Multi-Container Pod Design Lab 🚀

A hands-on Kubernetes lab focused on understanding and implementing Multi-Container Pods, including:

Sidecar Containers
Init Containers
Shared Volumes
Container Communication
Logging Architecture
Monitoring & Troubleshooting

This lab is ideal for beginners preparing for:

Kubernetes Administration
DevOps Engineering
Cloud-Native Development
CKAD Certification
Real-world Kubernetes Deployments
📚 Lab Objectives

By completing this lab, you will learn how to:

Create and manage multi-container pods
Use sidecar containers for logging and monitoring
Configure init containers for startup initialization
Share data between containers using volumes
Troubleshoot multi-container pod issues
Monitor logs and container interactions
Apply Kubernetes design patterns in practical scenarios
🛠 Prerequisites

Before starting this lab, you should have:

Basic Kubernetes knowledge
Familiarity with Pods and Containers
Understanding of YAML syntax
Basic Linux command-line experience
kubectl installed and configured
Minikube or Kubernetes cluster running
🧱 Project Structure
lab2-multicontainer/
│
├── multi-container-pod.yaml
├── advanced-multi-container.yaml
└── README.md
⚙️ Environment Setup

Create the project directory:

mkdir ~/lab2-multicontainer
cd ~/lab2-multicontainer

Verify Kubernetes cluster:

kubectl get nodes
🚀 Task 1: Multi-Container Pod with Sidecar Logging
Architecture

This pod contains:

Container Type	Purpose
Main Container	Runs Nginx web application
Sidecar Container	Reads and monitors logs
Init Container	Creates configuration files before app starts
📄 Create Pod Manifest

Create file:

nano multi-container-pod.yaml

Apply the manifest:

kubectl apply -f multi-container-pod.yaml

Verify pod:

kubectl get pods
🔍 Verify Init Container

Check init container logs:

kubectl logs multi-container-app -c config-setup

Expected output:

Initializing configuration...
Configuration setup complete!

Verify generated configuration:

kubectl exec multi-container-app -c web-app -- cat /etc/app-config/app.conf
🌐 Generate Web Traffic

Get pod IP:

kubectl get pod multi-container-app -o wide

Create temporary test client:

kubectl run test-client --image=busybox:1.35 --rm -it --restart=Never -- /bin/sh

Inside test client:

wget -q -O- http://POD_IP
wget -q -O- http://POD_IP/nonexistent
wget -q -O- http://POD_IP
exit
📜 Monitor Sidecar Logs

Check sidecar logs:

kubectl logs multi-container-app -c log-sidecar --tail=20

The sidecar container continuously monitors the nginx access logs using a shared volume.

📦 Shared Volume Verification

From main container:

kubectl exec multi-container-app -c web-app -- ls -la /var/log/nginx/

From sidecar container:

kubectl exec multi-container-app -c log-sidecar -- ls -la /var/log/nginx/
🚀 Task 2: Advanced Multi-Container Pod

Create advanced manifest:

nano advanced-multi-container.yaml

Deploy advanced pod:

kubectl apply -f advanced-multi-container.yaml
🧠 Advanced Architecture

This deployment contains:

Container	Role
data-initializer	Initializes shared data
main-app	Processes data
monitor-sidecar	Monitors shared files
cleanup-sidecar	Performs cleanup operations
📊 Monitor Containers

Check pod details:

kubectl describe pod advanced-multi-container

View logs:

Main App
kubectl logs advanced-multi-container -c main-app
Monitor Sidecar
kubectl logs advanced-multi-container -c monitor-sidecar
Cleanup Sidecar
kubectl logs advanced-multi-container -c cleanup-sidecar
🛠 Troubleshooting Commands
Describe Pod
kubectl describe pod multi-container-app
View Events
kubectl get events --field-selector involvedObject.name=multi-container-app
Check Resource Usage
kubectl top pod multi-container-app --containers
Access Specific Container Shell
kubectl exec -it multi-container-app -c web-app -- /bin/sh
🔄 Verify Container Communication

Write file from main container:

kubectl exec multi-container-app -c web-app -- sh -c 'echo "Hello from web-app" > /var/log/nginx/test-message.txt'

Read from sidecar container:

kubectl exec multi-container-app -c log-sidecar -- cat /var/log/nginx/test-message.txt
🧹 Cleanup Resources

Delete pods:

kubectl delete pod multi-container-app
kubectl delete pod advanced-multi-container

Verify cleanup:

kubectl get pods
📌 Key Kubernetes Concepts Learned
🔹 Init Containers

Init containers run before the main application containers start.

Use cases:

Configuration setup
Database initialization
Dependency checks
Secret preparation
🔹 Sidecar Containers

Sidecars provide helper functionality to the main application.

Common examples:

Log shipping
Monitoring
Metrics collection
Proxy services
🔹 Shared Volumes

Containers inside the same pod can share storage using volumes.

Example:

volumes:
- name: shared-data
  emptyDir: {}
📖 Best Practices

✅ Keep containers focused on a single responsibility
✅ Use init containers for setup tasks
✅ Use sidecars for monitoring and logging
✅ Share data through volumes instead of network calls
✅ Monitor resource usage regularly
✅ Use meaningful container names
✅ Keep YAML manifests version controlled

🧪 Useful Commands Cheat Sheet
# View all container logs
kubectl logs pod-name --all-containers=true

# Follow logs
kubectl logs -f pod-name -c container-name

# Execute shell
kubectl exec -it pod-name -c container-name -- /bin/sh

# Check mounts
kubectl describe pod pod-name | grep -A 5 "Mounts:"
🎯 Learning Outcome

After completing this lab, you will understand:

Multi-container pod architecture
Sidecar design pattern
Init container workflow
Shared volume communication
Kubernetes troubleshooting techniques
Real-world container collaboration patterns
📚 Real-World Use Cases

Multi-container pods are widely used in:

Microservices platforms
DevSecOps pipelines
Logging architectures
Service mesh deployments
Monitoring systems
Security scanning containers
🏆 CKAD Relevance

This lab directly supports concepts required for the:

Certified Kubernetes Application Developer (CKAD)

Important CKAD topics covered:

Multi-container Pods
Init Containers
Shared Volumes
Logging
Troubleshooting
🤝 Contributing

Feel free to fork this repository and improve the lab by:

Adding Helm charts
Adding Kubernetes Deployments
Adding ConfigMaps & Secrets
Adding Persistent Volumes
Integrating monitoring tools
⭐ Support

If this repository helped you learn Kubernetes, consider giving it a ⭐ on GitHub.

👨‍💻 Author

Zohaib Ahmed
DevOps | Cloud | Kubernetes | AI/ML Enthusiast

📜 License

This project is open-source and available under the MIT License.
