# Kubernetes MongoDB and Web Application Setup

This repository contains the Kubernetes configuration files to deploy a **MongoDB database** and a **web application** in a secure, scalable, and reliable manner. Perfect for learning Kubernetes fundamentals!

---

## 🚀 **Quick Start (For Learners)**

### **Prerequisites**

- [Docker](https://docs.docker.com/get-docker/) installed
- [Minikube](https://minikube.sigs.k8s.io/docs/start/) installed
- [kubectl](https://kubernetes.io/docs/tasks/tools/) installed

### **Step-by-Step Setup**

1. **Fork this repository** to your GitHub account
2. **Clone your fork locally:**

   ```bash
   git clone https://github.com/YOUR_USERNAME/K8S-DEMO-NANA.git
   cd K8S-DEMO-NANA
   ```

3. **Start Minikube:**

   ```bash
   minikube start
   ```

4. **Deploy everything:**

   ```bash
   kubectl apply -f .
   ```

5. **Check status:**

   ```bash
   kubectl get all
   kubectl get pods
   ```

6. **Access your application:**
   ```bash
   minikube service webapp-service
   ```

---

## 📋 **What You'll Learn**

This project demonstrates:

- ✅ **Deployments** - Running applications on Kubernetes
- ✅ **Services** - Networking between pods
- ✅ **Secrets** - Secure credential management
- ✅ **ConfigMaps** - Configuration management
- ✅ **Pod-to-Pod communication** - Internal networking
- ✅ **External access** - NodePort services

---

## 🏗️ **Architecture Overview**

### **Components**

1. **MongoDB Deployment**:

   - Deploys a MongoDB instance using the official `mongo:6.0.26` image
   - Configures the database with secure credentials stored in a Kubernetes Secret
   - Runs on port 27017 internally

2. **MongoDB Service**:

   - Exposes the MongoDB instance internally within the Kubernetes cluster on port **27017**
   - Allows other pods to communicate with the database
   - Type: ClusterIP (internal access only)

3. **Web Application Deployment**:

   - Deploys a web application using the `nanajanashia/k8s-demo-app:v1.0` image
   - The application listens on port **3000**
   - Fetches MongoDB credentials and service URL dynamically using Kubernetes Secrets and ConfigMaps

4. **Web Application Service**:

   - Exposes the web application externally via NodePort on port **30100**
   - Allows external access to the web application
   - Type: NodePort (external access)

5. **Secrets and ConfigMap**:
   - **Secrets**: Securely store sensitive data like MongoDB credentials
   - **ConfigMap**: Store non-sensitive configuration data, such as the MongoDB service URL

---

## 🗺️ **Architecture Diagram**

```plaintext
+-------------------+       +-------------------+
|   mongo-secret    |       |   mongo-config    |
|  (Kubernetes)     |       |  (Kubernetes)     |
|-------------------|       |-------------------|
| Stores MongoDB    |       | Stores the MongoDB|
| credentials       |       | service URL       |
+-------------------+       +-------------------+

         |                           |
         |                           |
         v                           v

+---------------------------------------------------+
|                 MongoDB Deployment                |
|---------------------------------------------------|
| - Deploys MongoDB pod using the mongo:6.0.26     |
| - Uses credentials from mongo-secret             |
| - Exposes port 27017                             |
+---------------------------------------------------+
                          |
                          |
                          v
+---------------------------------------------------+
|                 MongoDB Service                  |
|---------------------------------------------------|
| - Exposes MongoDB internally on port 27017       |
| - Allows other pods to connect to the database   |
+---------------------------------------------------+

         ^
         |
         |
+-------------------+       +-------------------+
|   Web Application |       |   WebApp Service  |
|   Deployment      |       |-------------------|
|-------------------|       | - Exposes the web |
| - Deploys the app |       |   app on port 3000|
|   on port 3000    |       | - Routes traffic  |
| - Uses the MongoDB|       |   to the app pod  |
|   service URL     |       | - External access |
| - Fetches DB URL  |       |   via NodePort    |
|   from ConfigMap  |       |   30100           |
| - Fetches USER    |       +-------------------+
|   credentials     |
|   from Secret     |
+-------------------+
```

---

## 🔧 **File Structure**

```
K8S-DEMO-NANA/
├── mongo-config.yaml      # MongoDB connection configuration
├── mongo-secret.yaml      # MongoDB credentials (base64 encoded)
├── mongo.yaml            # MongoDB deployment and service
├── webapp.yaml           # Web application deployment and service
└── ReadMe.md             # This file
```

---

## 📖 **Detailed Instructions**

### **1. Understanding the YAML Files**

#### **mongo-secret.yaml**

- Creates a Kubernetes Secret to store MongoDB username and password
- Values are base64 encoded for security
- Referenced by the MongoDB deployment

#### **mongo-config.yaml**

- Creates a ConfigMap with the MongoDB service URL
- Referenced by the web application deployment

#### **mongo.yaml**

- **Deployment**: Runs MongoDB container with credentials from secret
- **Service**: Exposes MongoDB internally on port 27017

#### **webapp.yaml**

- **Deployment**: Runs web application with environment variables from secret and config
- **Service**: Exposes web app externally on NodePort 30100

### **2. Deployment Commands**

```bash
# Deploy all resources
kubectl apply -f .

# Deploy individual files
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongo-config.yaml
kubectl apply -f mongo.yaml
kubectl apply -f webapp.yaml
```

### **3. Verification Commands**

```bash
# Check all resources
kubectl get all

# Check pods status
kubectl get pods

# Check services
kubectl get services

# Check secrets and configmaps
kubectl get secrets
kubectl get configmaps

# View pod logs
kubectl logs -l app=webapp
kubectl logs -l app=mongo

# Describe resources for debugging
kubectl describe pod <pod-name>
kubectl describe service <service-name>
```

### **4. Accessing Your Application**

```bash
# Get Minikube IP
minikube ip

# Access web app directly (replace <minikube-ip> with actual IP)
curl http://<minikube-ip>:30100

# Open in browser using Minikube service
minikube service webapp-service

# Port forward for direct access
kubectl port-forward service/webapp-service 8080:3000
# Then visit http://localhost:8080
```

---

## 🧪 **Learning Exercises**

Try these modifications to learn more:

1. **Scale the web application:**

   ```bash
   kubectl scale deployment webapp-deployment --replicas=3
   ```

2. **Update the MongoDB image:**

   ```bash
   kubectl set image deployment/mongo-deployment mongodb=mongo:7.0
   ```

3. **View real-time logs:**

   ```bash
   kubectl logs -f -l app=webapp
   ```

4. **Execute commands in pods:**

   ```bash
   kubectl exec -it <pod-name> -- /bin/bash
   ```

5. **Delete and recreate resources:**
   ```bash
   kubectl delete -f .
   kubectl apply -f .
   ```

---

## 🚨 **Troubleshooting**

### **Common Issues**

1. **Pods not starting:**

   ```bash
   kubectl describe pod <pod-name>
   kubectl get events --sort-by='.lastTimestamp'
   ```

2. **Service not accessible:**

   ```bash
   kubectl get endpoints
   kubectl describe service <service-name>
   ```

3. **Image pull errors:**

   ```bash
   kubectl describe pod <pod-name>
   # Check if image exists and is accessible
   ```

4. **Minikube not starting:**
   ```bash
   minikube delete
   minikube start
   ```

### **Useful Debugging Commands**

```bash
# Check cluster info
kubectl cluster-info

# Check node status
kubectl get nodes

# Check Minikube status
minikube status

# View all resources in namespace
kubectl get all -o wide
```

---

## 🧹 **Cleanup**

When you're done learning:

```bash
# Delete all resources
kubectl delete -f .

# Stop Minikube
minikube stop

# Delete Minikube cluster (optional)
minikube delete
```

---

## 📚 **Next Steps**

After mastering this setup, explore:

- **Ingress Controllers** - Advanced routing
- **Persistent Volumes** - Data persistence
- **Helm Charts** - Package management
- **Multi-node clusters** - Production scaling
- **Monitoring & Logging** - Observability
- **Security Policies** - RBAC and network policies

---

## 🤝 **Contributing**

Feel free to:

- Fork this repository
- Create issues for bugs or improvements
- Submit pull requests with enhancements
- Share your learning experiences

---

## 📄 **License**

This project is open source and available under the [MIT License](LICENSE).

---

## 🙏 **Acknowledgments**

- Built for Kubernetes learning and experimentation
- Uses official MongoDB and custom demo application images
- Designed to demonstrate real-world Kubernetes concepts

**Happy Learning! 🚀**
