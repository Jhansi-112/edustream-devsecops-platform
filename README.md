<div align="center">

# 🚀 EduStream DevSecOps Platform

### End-to-End CI/CD Pipeline · Kubernetes on AWS EC2 · Trivy Security · Prometheus · Grafana

[![Jenkins](https://img.shields.io/badge/Jenkins-CI/CD-D24939?style=for-the-badge&logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.29-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/DockerHub-jhansi977-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://hub.docker.com/)
[![Trivy](https://img.shields.io/badge/Trivy-Security_Scan-1904DA?style=for-the-badge&logo=aqua&logoColor=white)](https://trivy.dev/)
[![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-Dashboards-F46800?style=for-the-badge&logo=grafana&logoColor=white)](https://grafana.com/)
[![AWS](https://img.shields.io/badge/AWS-EC2-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)](https://aws.amazon.com/)

</div>

---

## 🌟 About This Project

This is a **real-time, hands-on DevSecOps project** built completely from scratch on AWS EC2.

A microservices-based e-commerce application is deployed on a manually provisioned Kubernetes cluster — automated end-to-end using Jenkins CI/CD, secured with Trivy image scanning, and monitored with Prometheus and Grafana.

> 💬 *Every error in this project is real. Every fix is real. This is not a tutorial copy — this is built from scratch.*

---

## 🏗️ Architecture Overview

```
Developer (git push)
        │
        ▼
   GitHub Repo ──── webhook trigger
        │
        ▼
┌─────────────────────────────────────────────┐
│         Jenkins CI/CD — AWS EC2             │
│  Build #16 · Total time: 51 sec · All ✔    │
│                                             │
│  Stage 1: Clone repo       ✓  0.85s        │
│  Stage 2: docker build     ✓  1s           │
│  Stage 3: docker push      ✓  5s           │
│  Stage 4: Trivy Scan       ✓  37s          │
│  Stage 5: kubectl apply    ✓  3s           │
└───────────────────┬─────────────────────────┘
                    │
                    ▼
         DockerHub Registry
         jhansi977/edustream:v1
                    │
         ┌──────────┘ image pull
         ▼
┌─────────────────────────────────────────────┐
│     Kubernetes Cluster — AWS EC2            │
│                                             │
│  ┌─────────────┐    ┌─────────────────────┐ │
│  │ Master Node │    │    Worker Node      │ │
│  │             │    │  (all pods here)    │ │
│  │ API Server  │    │                     │ │
│  │ Scheduler   │───▶│  frontend :30093    │ │
│  │ etcd        │    │  cartservice        │ │
│  │ Controller  │    │  checkoutservice    │ │
│  │ Calico CNI  │    │  paymentservice     │ │
│  │ Metrics Svr │    │  productcatalog     │ │
│  └─────────────┘    │  shippingservice    │ │
│                     │  emailservice       │ │
│                     │  recommendation     │ │
│                     │  currencyservice    │ │
│                     │  adservice         │ │
│                     │  redis-cart        │ │
│                     └─────────────────────┘ │
└───────────────────┬─────────────────────────┘
                    │ NodePort :30093
                    ▼
             User Browser
                    │
                    ▼
┌─────────────────────────────────────────────┐
│         Monitoring Stack                    │
│                                             │
│  Metrics Server ──scrape 15s──▶ Prometheus  │
│  Node Exporter  ──────────────▶ (port 9090) │
│  Pod /metrics   ──────────────▶     │       │
│                                     │PromQL │
│                                     ▼       │
│                               Grafana :3000 │
│                          CPU · Memory · Pods│
└─────────────────────────────────────────────┘
```

---

## 🛠️ Tools & Technologies

| Category | Tool |
|---|---|
| Cloud Infrastructure | AWS EC2 (Ubuntu 22.04) |
| Container Runtime | Docker + cri-dockerd |
| Orchestration | Kubernetes v1.29 (kubeadm) |
| Pod Networking | Calico CNI |
| CI/CD | Jenkins 2.541 |
| Image Registry | DockerHub (jhansi977/edustream:v1) |
| Source Control | GitHub + Webhook trigger |
| Security Scanning | Trivy (HIGH + CRITICAL severity) |
| Monitoring | Prometheus (Helm install) |
| Visualization | Grafana v12 |
| Resource Metrics | Kubernetes Metrics Server |

---

## ☁️ AWS Infrastructure — 3 EC2 Servers

| Server | Purpose |
|---|---|
| `devops-server` | Jenkins + Docker + kubectl |
| `k8s-master` | Kubernetes Control Plane |
| `k8s-worker` | Runs all microservices (all pods) |

### AWS Security Group — Open Ports

| Port | Service |
|---|---|
| 22 | SSH |
| 80 | HTTP |
| 8080 | Jenkins |
| 6443 | Kubernetes API |
| 3000 | Grafana |
| 9090 | Prometheus |
| 30000–32767 | Kubernetes NodePorts |

---

## 📦 Microservices (13 Total — All on Worker Node)

| Service | What it does | Type |
|---|---|---|
| **frontend** | User interface — exposed on NodePort :30093 | Frontend |
| **redis-cart** | Session/cart data storage | Storage |
| **cartservice** | Reads and writes to redis | Microservice |
| **checkoutservice** | Handles payment + shipping flow | Microservice |
| **paymentservice** | Processes transactions | Microservice |
| **shippingservice** | Calculates shipping cost | Microservice |
| **emailservice** | Sends order confirmation emails | Microservice |
| **productcatalogservice** | Lists available products | Microservice |
| **recommendationservice** | Suggests related products | Microservice |
| **currencyservice** | FX / currency conversion | Microservice |
| **adservice** | Shows contextual ads | Microservice |
| **loadgenerator** | Simulates user traffic | Load Testing |

---

## 🔁 Jenkins CI/CD Pipeline (Jenkinsfile)

```groovy
pipeline {
    agent any
    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Jhansi-112/edustream-devsecops-platform.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jhansi977/edustream:v1 .'
            }
        }
        stage('Push to DockerHub') {
            steps {
                sh 'docker push jhansi977/edustream:v1'
            }
        }
        stage('Trivy Security Scan') {
            steps {
                sh 'trivy image --exit-code 0 --severity HIGH,CRITICAL --format table jhansi977/edustream:v1'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f release/kubernetes-manifests.yaml'
            }
        }
    }
}
```

### Build Stats
| What | Result |
|---|---|
| Total Build Time | 51 seconds |
| Build Number | #16 |
| Stage 1 — Clone | ✅ 0.85s |
| Stage 2 — Docker Build | ✅ 1s |
| Stage 3 — Docker Push | ✅ 5s |
| Stage 4 — Trivy Scan | ✅ 37s |
| Stage 5 — K8s Deploy | ✅ 3s |

---

## 🔍 Trivy Security Scan Results

Trivy scans the Docker image on every build for HIGH and CRITICAL CVEs.

```bash
trivy image --exit-code 0 --severity HIGH,CRITICAL \
--format table jhansi977/edustream:v1
```

| Target | Result |
|---|---|
| jhansi977/edustream:v1 (debian 13.4) | 17 vulnerabilities found |
| node-pkg: currencyservice | ✅ 0 CVEs (clean) |
| node-pkg: paymentservice | ✅ 0 CVEs (clean) |

> ⚠️ `--exit-code 0` is used — pipeline **does not block** on vulnerabilities. Scan runs as a visibility/reporting step. Build #16 passed ✅

---

## 📊 Monitoring Stack

### How It Works
```
Metrics Server  ─┐
Node Exporter   ─┼──scrape every 15s──▶ Prometheus (port 9090)
Pod /metrics    ─┘                              │
                                           PromQL queries
                                                │
                                                ▼
                                       Grafana (port 3000)
                                   CPU · Memory · Pod dashboards
```

### Prometheus Targets — All UP ✅
| Target | Status |
|---|---|
| Grafana | ✅ UP |
| Alertmanager | ✅ UP |
| Kubernetes API Server | ✅ UP |
| CoreDNS | ✅ UP |
| Node Exporter | ✅ UP |

### Live Node Metrics (kubectl top nodes)
```
NAME         CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
k8s-master   184m         9%     2169Mi          28%
k8s-worker   426m         21%    2547Mi          33%
```

### Grafana Dashboards
- 📊 CPU dashboard — node + pod usage
- 📊 Memory dashboard — requests vs limits
- 📊 Pod-level view — throttling + container details
- 📊 Live metrics — Master 184m · Worker 426m

---

## 🧠 Real Problems I Solved

These are actual production-level issues I debugged and fixed:

| Problem | Root Cause | Fix |
|---|---|---|
| `CRI runtime not running` | containerd misconfigured | Reinstalled with Docker repo + cri-dockerd |
| Calico pods `0/1 Running` | BGP not established between nodes | Added All-Traffic inbound rule (same SG to same SG) |
| DNS failure between microservices | Calico not ready → CoreDNS failing | Fixed networking + restarted CoreDNS |
| Jenkins `permission denied` on Docker | Jenkins user not in Docker group | `sudo usermod -aG docker jenkins` |
| Grafana showing `No Data` | Metrics Server not configured | Reinstalled + added `--kubelet-insecure-tls` flag |
| `kubectl top` not working | Wrong args in metrics-server YAML | Fixed deployment args + restarted |

---

## 🚀 How to Practice This Project Yourself

### Step 1 — Clone this repo
```bash
git clone https://github.com/Jhansi-112/edustream-devsecops-platform.git
cd edustream-devsecops-platform
```

### Step 2 — Create 3 EC2 instances on AWS
```
devops-server → Ubuntu 22.04, 30GB
k8s-master    → Ubuntu 22.04, 30GB
k8s-worker    → Ubuntu 22.04, 30GB
```

### Step 3 — Install Docker on all servers
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y docker.io
sudo systemctl enable docker && sudo systemctl start docker
```

### Step 4 — Install Kubernetes (Master + Worker)
```bash
sudo apt install -y apt-transport-https ca-certificates curl
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | \
sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | \
sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

### Step 5 — Initialize Kubernetes Master
```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16 \
--cri-socket=unix:///var/run/cri-dockerd.sock

mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Step 6 — Install Calico Network
```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.2/manifests/calico.yaml
```

### Step 7 — Join Worker Node
```bash
sudo kubeadm join <MASTER-IP>:6443 \
--token <token> \
--discovery-token-ca-cert-hash sha256:<hash> \
--cri-socket=unix:///var/run/cri-dockerd.sock
```

### Step 8 — Deploy All Microservices
```bash
kubectl apply -f release/kubernetes-manifests.yaml
kubectl get pods
kubectl get svc frontend-external
```

### Step 9 — Install Jenkins (DevOps Server)
```bash
sudo apt install openjdk-17-jdk -y
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update && sudo apt install jenkins -y
sudo systemctl start jenkins
```

### Step 10 — Install Prometheus + Grafana
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack
kubectl patch svc monitoring-grafana -p '{"spec": {"type": "NodePort"}}'
kubectl patch svc monitoring-kube-prometheus-prometheus -p '{"spec": {"type": "NodePort"}}'
```

### Step 11 — Access Everything
```
Application  →  http://<WORKER-IP>:30093
Jenkins      →  http://<DEVOPS-IP>:8080
Prometheus   →  http://<MASTER-IP>:9090
Grafana      →  http://<MASTER-IP>:3000
```

---

## 🌐 Project Portfolio

👉 Architecture Diagram → [edustream-devsecops-architecture-updated.html](./edustream-devsecops-architecture-updated.html)

👉 Full Project Portfolio → [edustream-devops-portfolio.html](./edustream-devops-portfolio.html)

---

<div align="center">

**Built from scratch by Jhansi 👩‍💻**

**⭐ Star this repo if it helped you learn DevSecOps!**

[![GitHub](https://img.shields.io/badge/GitHub-Jhansi--112-181717?style=for-the-badge&logo=github)](https://github.com/Jhansi-112)

</div>
