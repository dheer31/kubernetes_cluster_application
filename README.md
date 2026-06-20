# 🚀 Kubernetes Cluster Application (Maven + Docker + Kubernetes)

## 📌 Project Overview

This project demonstrates a complete **end-to-end deployment pipeline** for a Java web application using:

* **Maven** → Build tool
* **Docker** → Containerization
* **Tomcat** → Application server
* **Kubernetes (kubeadm cluster)** → Deployment & orchestration
* **GitHub** → Version control

---

## 🏗️ Tech Stack

* Java 17
* Maven
* Apache Tomcat 10
* Docker
* Kubernetes (Master + Worker setup)

---

## ⚙️ Step 1: Create Maven Project

```bash
mvn archetype:generate
```

Provide:

* **GroupId** → amazon
* **ArtifactId** → amazon

Open project:

```bash
code .
```

---

## 🐳 Step 2: Dockerfile

```dockerfile
# ----------- STAGE 1: BUILD -----------
FROM maven:3.9.9-eclipse-temurin-17 AS builder
WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline

COPY src ./src
RUN mvn clean package -DskipTests

# ----------- STAGE 2: TOMCAT -----------
FROM tomcat:10.1-jdk17

RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=builder /app/target/*.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]
```

---

## ✏️ Step 3: Modify Web Page

Rename:

```
index.jsp → index.html
```

---

## 📤 Step 4: Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin <your-repo-url>
git push -u origin master
```

---

## 🐳 Step 5: Docker Server Setup

```bash
apt update
apt install docker.io -y

docker --version
mvn --version
```

Clone repo:

```bash
git clone https://github.com/Thanuu55/kubernetes_cluster_application.git
cd kubernetes_cluster_application
```

Install Java:

```bash
apt install openjdk-17-jre-headless -y
```

Build project:

```bash
mvn clean package
```

---

## 🐳 Step 6: Build & Push Docker Image

```bash
docker build -t kubernetes_cluster_application .
docker images

docker login -u thanushrii

docker tag kubernetes_cluster_application thanushrii/kubernetes_cluster_application
docker push thanushrii/kubernetes_cluster_application
```

---

## ☸️ Step 7: Kubernetes Setup

### 🔹 Master Node

```bash
sudo su -
apt update
curl -sL https://tinyurl.com/mt346aen | bash
```

Join command (example):

```bash
kubeadm join 172.31.36.223:6443 --token 4a21g4.b3h2c7zj3izavtjf \
--discovery-token-ca-cert-hash sha256:57785298e9dc7155093daaed5cae581f0dd4e983e495e9c732b94c5417b40629
```

Apply deployment:

```bash
kubectl apply -f thanu.yml
```

Delete deployment:

```bash
kubectl delete -f thanu.yml
```

---

### 🔹 Worker Node

```bash
sudo su -
apt update
curl -sL https://tinyurl.com/yvmvxmzb | bash
```

Join cluster:

```bash
kubeadm join 172.31.36.223:6443 --token 4a21g4.b3h2c7zj3izavtjf \
--discovery-token-ca-cert-hash sha256:57785298e9dc7155093daaed5cae581f0dd4e983e495e9c732b94c5417b40629
```

---

## 📄 Sample Kubernetes YAML (`thanu.yml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: amazon-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: amazon
  template:
    metadata:
      labels:
        app: amazon
    spec:
      containers:
      - name: amazon-container
        image: thanushrii/kubernetes_cluster_application
        ports:
        - containerPort: 8080
```

---

## 🌐 Access Application

```bash
kubectl get pods
kubectl get svc
```

Expose service:

```bash
kubectl expose deployment amazon-app --type=NodePort --port=8080
```

---

## ✅ Key Concepts Covered

* Maven project setup
* Multi-stage Docker build
* Docker image push to Docker Hub
* Kubernetes cluster (Master + Worker)
* Deployment using YAML
* Scaling using replicas

---

## 📌 Author

DHEER31
---
