# Kubernetes MongoDB and Web Application Setup

This repository contains the Kubernetes configuration files to deploy a **MongoDB database** and a **web application** in a secure, scalable, and reliable manner.

---

## **Overview**

### **Components**

1. **MongoDB Deployment**:

   - Deploys a MongoDB instance using the official `mongo:6.0.26` image.
   - Configures the database with secure credentials stored in a Kubernetes Secret.

2. **MongoDB Service**:

   - Exposes the MongoDB instance internally within the Kubernetes cluster on port **27017**.
   - Allows other pods to communicate with the database.

3. **Web Application Deployment**:

   - Deploys a web application using the `nanajanashia/k8s-demo-app:v1.0` image.
   - The application listens on port **3000**.
   - Fetches MongoDB credentials and service URL dynamically using Kubernetes Secrets and ConfigMaps.

4. **Web Application Service**:

   - Exposes the web application internally within the Kubernetes cluster on port **3000**.
   - Allows other pods to communicate with the web application.

5. **Secrets and ConfigMap**:
   - **Secrets**: Securely store sensitive data like MongoDB credentials.
   - **ConfigMap**: Store non-sensitive configuration data, such as the MongoDB service URL.

---

## **Architecture Diagram**

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
|   service URL     |       +-------------------+
| - Fetches DB URL  |
|   from ConfigMap  |
| - Fetches USER    |
|   credentials     |
|   from Secret     |
+-------------------+
```
