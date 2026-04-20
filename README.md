<div align="center">

![header](https://capsule-render.vercel.app/api?type=waving&color=0:27500A,100:72243E&height=200&section=header&text=EduStream%20DevSecOps%20Platform&fontSize=38&fontColor=ffffff&fontAlignY=38&desc=Production-Grade%20DevSecOps%20Pipeline%20on%20Kubernetes%20%7C%20AWS%20EC2%20%7C%20Jenkins%20%7C%20Docker%20%7C%20Trivy%20%7C%20Prometheus%20%7C%20Grafana&descSize=13&descAlignY=58&descColor=C0DD97)

<br/>

[![Jenkins](https://img.shields.io/badge/Jenkins-27500A?style=for-the-badge&logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-72243E?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-27500A?style=for-the-badge&logo=docker&logoColor=white)](https://hub.docker.com/)
[![Trivy](https://img.shields.io/badge/Trivy-72243E?style=for-the-badge&logo=aqua&logoColor=white)](https://trivy.dev/)
[![Prometheus](https://img.shields.io/badge/Prometheus-27500A?style=for-the-badge&logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-72243E?style=for-the-badge&logo=grafana&logoColor=white)](https://grafana.com/)
[![AWS](https://img.shields.io/badge/AWS%20EC2-27500A?style=for-the-badge&logo=amazonaws&logoColor=white)](https://aws.amazon.com/)

<br/>

<table>
<tr>
<td align="center">
<a href="https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html">
<div style="border:2px solid #27500A;border-radius:10px;overflow:hidden;display:inline-block;min-width:200px;">
<div style="background:#27500A;padding:6px 20px;text-align:center;">
<span style="color:#C0DD97;font-size:11px;font-weight:700;letter-spacing:2px;">✨ CLICK TO VIEW ✨</span>
</div>
<div style="padding:12px 20px;text-align:center;background:#f0faf0;">
<span style="color:#27500A;font-size:15px;font-weight:800;">🚀 LIVE PORTFOLIO</span>
</div>
</div>
</a>
</td>
<td width="30"></td>
<td align="center">
<a href="https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devsecops-architecture.html">
<div style="border:2px solid #72243E;border-radius:10px;overflow:hidden;display:inline-block;min-width:200px;">
<div style="background:#72243E;padding:6px 20px;text-align:center;">
<span style="color:#F4C0D1;font-size:11px;font-weight:700;letter-spacing:2px;">✨ CLICK TO VIEW ✨</span>
</div>
<div style="padding:12px 20px;text-align:center;background:#fff0f5;">
<span style="color:#72243E;font-size:15px;font-weight:800;">🏗️ ARCHITECTURE</span>
</div>
</div>
</a>
</td>
</tr>
</table>

</div>

<hr style="border:2px solid #27500A;border-radius:2px;">

## <span style="color:#27500A">📋 Table of Contents</span>

- [Project Overview](#-project-overview)
- [Architecture](#-architecture)
- [Key Features](#-key-features)
- [Pre-Requisites](#-pre-requisites)
- [Infrastructure Setup](#-infrastructure-setup)
- [Kubernetes Cluster Setup](#-kubernetes-cluster-setup)
- [Application Deployment](#-application-deployment)
- [Jenkins CI/CD Pipeline](#-jenkins-cicd-pipeline)
- [Security Scanning with Trivy](#-security-scanning-with-trivy)
- [Monitoring Setup](#-monitoring-setup)
- [Project Stats](#-project-stats)
- [Troubleshooting Guide](#-troubleshooting-guide)
- [Contributing](#-contributing)

<hr style="border:2px solid #72243E;border-radius:2px;">

## <span style="color:#72243E">🌟 Project Overview</span>

### Introduction

This project demonstrates the deployment of a **production-grade DevSecOps platform** built completely from scratch on AWS EC2. A microservices-based e-commerce application is deployed on a manually provisioned Kubernetes cluster — automated end-to-end using Jenkins CI/CD, secured with Trivy container scanning, and monitored with Prometheus and Grafana.

> 💬 *Every command in this project was run by me. Every error was faced by me. Every fix was figured out by me. This is not a tutorial copy.*

### Key Features

> [!NOTE]
> All features below are implemented and verified in this project.

- 🟢 **End-to-End Automation** — Every `git push` triggers the full pipeline automatically via GitHub webhook
- 🩷 **Container Security** — Trivy scans every Docker image for HIGH and CRITICAL CVEs before deployment
- 🟢 **Manual Kubernetes** — Cluster bootstrapped using `kubeadm` — no EKS or managed services
- 🩷 **Full Observability** — Prometheus scrapes metrics every 15 seconds, visualized in Grafana dashboards
- 🟢 **Real Debugging** — Real production errors faced and solved during this project
- 🩷 **18 Days Uptime** — 13 microservices running continuously without interruption

<hr style="border:2px solid #27500A;border-radius:2px;">

## <span style="color:#27500A">🏗️ Architecture</span>

### Architecture Diagram

```
  Developer
     │
     │  git push
     ▼
  ┌──────────────────┐
  │    GitHub Repo   │──── webhook trigger ──────────────────────────┐
  └──────────────────┘                                               │
                                                                     ▼
                                              ┌──────────────────────────────────┐
                                              │    Jenkins Server  ·  AWS EC2    │
                                              │                                  │
                                              │  Stage 1 ── Clone Repo   0.85s  │
                                              │  Stage 2 ── Docker Build   1s   │
                                              │  Stage 3 ── Push DockerHub  5s  │
                                              │  Stage 4 ── Trivy Scan    37s   │
                                              │  Stage 5 ── kubectl apply   3s  │
                                              └──────────────┬───────────────────┘
                                                             │
                                    ┌────────────────────────┘
                                    │
                     ┌──────────────▼──────────────┐
                     │         DockerHub            │
                     │    jhansi977/edustream:v1    │
                     └──────────────┬──────────────┘
                                    │  image pull
                                    ▼
           ┌────────────────────────────────────────────────────┐
           │          Kubernetes Cluster  ·  AWS EC2            │
           │  ┌──────────────────┐    ┌──────────────────────┐  │
           │  │   Master Node    │    │    Worker Node       │  │
           │  │  API Server      │───▶│  frontend  :30093    │  │
           │  │  Scheduler       │    │  cartservice         │  │
           │  │  etcd            │    │  redis-cart          │  │
           │  │  Controller Mgr  │    │  checkoutservice     │  │
           │  │  Calico CNI      │    │  paymentservice      │  │
           │  │  Metrics Server  │    │  shippingservice     │  │
           │  └──────────────────┘    │  emailservice        │  │
           │                         │  productcatalog       │  │
           │                         │  recommendation       │  │
           │                         │  currencyservice      │  │
           │                         │  adservice            │  │
           │                         │  loadgenerator        │  │
           │                         └──────────────────────┘  │
           └─────────────────────────────┬──────────────────────┘
                                         │  NodePort :30093
                                         ▼
                                    User Browser
                                         ▼
           ┌──────────────────────────────────────────┐
           │           Monitoring Stack               │
           │  Prometheus (9090) ──▶ Grafana (3000)   │
           │  scrapes: Metrics Server · Node Exporter │
           └──────────────────────────────────────────┘
```

### Infrastructure Components

> [!TIP]
> Each layer below works together to form the complete DevSecOps pipeline.

**🟢 CI/CD Layer**
- Jenkins on AWS EC2 — automated 5-stage pipeline
- GitHub webhooks — auto-triggers on every push
- DockerHub — stores built images

**🩷 Container Orchestration**
- Kubernetes v1.29 — bootstrapped using kubeadm
- Calico CNI — pod-to-pod networking
- cri-dockerd — container runtime interface

**🟢 Application Layer**
- 13 microservices running on Worker Node
- Frontend exposed via NodePort :30093
- Services communicate via Kubernetes DNS

**🩷 Monitoring Layer**
- Prometheus — metrics collection every 15s
- Grafana — CPU, memory, pod dashboards
- Metrics Server — enables kubectl top

<hr style="border:2px solid #72243E;border-radius:2px;">

## <span style="color:#72243E">✅ Key Features</span>

| Feature | Details |
|:---|:---|
| 🟢 Automated CI/CD | GitHub push → Jenkins → Docker → K8s in 51 seconds |
| 🩷 Security Scanning | Trivy scans every image — HIGH + CRITICAL severity |
| 🟢 Manual Kubernetes | kubeadm cluster — no cloud-managed services |
| 🩷 Real-Time Monitoring | Prometheus + Grafana — live dashboards |
| 🟢 13 Microservices | All pods running on worker node |
| 🩷 18 Days Uptime | Continuous operation without interruption |

<hr style="border:2px solid #27500A;border-radius:2px;">

## <span style="color:#27500A">🔧 Pre-Requisites</span>

### Required Accounts

| Account | Purpose |
|:---|:---|
| 🟢 AWS Account | EC2 instance hosting |
| 🩷 GitHub Account | Source code repository |
| 🟢 DockerHub Account | Container image registry |

### AWS EC2 Instances Required

| Server | Purpose | OS | Storage |
|:---|:---|:---|:---|
| 🟢 `devops-server` | Jenkins + Docker + kubectl | Ubuntu 22.04 | 30GB |
| 🩷 `k8s-master` | Kubernetes Control Plane | Ubuntu 22.04 | 30GB |
| 🟢 `k8s-worker` | Runs all microservices | Ubuntu 22.04 | 30GB |

### Required Tools

```bash
# On all servers
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl wget git vim net-tools unzip

# Verify
git --version
curl --version
```

### Security Group Configuration

> [!WARNING]
> The "All Traffic from same SG" rule is critical. Without it, Calico BGP will not establish and pod DNS will fail.

```
Port 22          →  SSH
Port 80          →  HTTP
Port 8080        →  Jenkins UI
Port 6443        →  Kubernetes API Server
Port 3000        →  Grafana Dashboard
Port 9090        →  Prometheus UI
Port 30000-32767 →  Kubernetes NodePorts
All Traffic      →  Same Security Group (node-to-node communication)
```

<hr style="border:2px solid #72243E;border-radius:2px;">

## <span style="color:#72243E">☁️ Infrastructure Setup</span>

### Step 1 — Launch EC2 Instances

Create all 3 instances with:
- AMI: Ubuntu 22.04 LTS
- Instance Type: m7i-flex.large
- Storage: 30GB
- Security Group: same group for all 3

### Step 2 — Install Docker (All 3 Servers)

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker ubuntu
docker --version
```

### Step 3 — Install cri-dockerd (Master + Worker)

```bash
wget https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.10/cri-dockerd_0.3.10.3-0.ubuntu-jammy_amd64.deb
sudo dpkg -i cri-dockerd_0.3.10.3-0.ubuntu-jammy_amd64.deb
sudo systemctl enable cri-docker
sudo systemctl start cri-docker
sudo systemctl status cri-docker
```

<hr style="border:2px solid #27500A;border-radius:2px;">

## <span style="color:#27500A">☸️ Kubernetes Cluster Setup</span>

### Step 1 — Install Kubernetes Packages (Master + Worker)

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

### Step 2 — Set Hostnames

```bash
# On master
sudo hostnamectl set-hostname k8s-master

# On worker
sudo hostnamectl set-hostname k8s-worker
```

### Step 3 — Initialize Kubernetes Master

```bash
sudo kubeadm init \
  --pod-network-cidr=192.168.0.0/16 \
  --cri-socket=unix:///var/run/cri-dockerd.sock

mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl get nodes
```

### Step 4 — Install Calico Network Plugin

```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.2/manifests/calico.yaml
kubectl get pods -n kube-system
```

### Step 5 — Join Worker Node

```bash
sudo kubeadm join <MASTER-IP>:6443 \
  --token <token> \
  --discovery-token-ca-cert-hash sha256:<hash> \
  --cri-socket=unix:///var/run/cri-dockerd.sock

# Verify on master
kubectl get nodes
# k8s-master   Ready    control-plane   v1.29.15
# k8s-worker   Ready    <none>          v1.29.15
```

<hr style="border:2px solid #72243E;border-radius:2px;">

## <span style="color:#72243E">📦 Application Deployment</span>

### Microservices Overview

| # | 🟢 Service | Role |
|:---:|:---|:---|
| 1 | frontend | User interface — NodePort :30093 |
| 2 | cartservice | Shopping cart management |
| 3 | redis-cart | Cart data storage |
| 4 | checkoutservice | Order processing |
| 5 | paymentservice | Payment transactions |
| 6 | shippingservice | Shipping cost calculation |
| 7 | emailservice | Order confirmation emails |
| 8 | productcatalogservice | Product listings |
| 9 | recommendationservice | Product suggestions |
| 10 | currencyservice | Currency conversion |
| 11 | adservice | Contextual advertisements |
| 12 | loadgenerator | Traffic simulation |

### Deploy All Services

```bash
git clone https://github.com/Jhansi-112/edustream-devsecops-platform.git
cd edustream-devsecops-platform

kubectl apply -f release/kubernetes-manifests.yaml

kubectl get pods
kubectl get svc frontend-external
```

### Access the Application

```bash
# Open in browser
http://<WORKER-PUBLIC-IP>:30093
```

<hr style="border:2px solid #27500A;border-radius:2px;">

## <span style="color:#27500A">🔁 Jenkins CI/CD Pipeline</span>

### Step 1 — Install Jenkins

```bash
sudo apt install openjdk-17-jdk -y
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update && sudo apt install jenkins -y
sudo systemctl start jenkins
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### Step 2 — Configure Jenkins Access

```bash
sudo usermod -aG docker jenkins
sudo mkdir -p /var/lib/jenkins/.kube
sudo cp /root/.kube/config /var/lib/jenkins/.kube/config
sudo chown -R jenkins:jenkins /var/lib/jenkins/.kube
sudo systemctl restart jenkins
```

### Step 3 — Jenkinsfile

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
            steps { sh 'docker build -t jhansi977/edustream:v1 .' }
        }
        stage('Push to DockerHub') {
            steps { sh 'docker push jhansi977/edustream:v1' }
        }
        stage('Trivy Security Scan') {
            steps { sh 'trivy image --exit-code 0 --severity HIGH,CRITICAL jhansi977/edustream:v1' }
        }
        stage('Deploy to Kubernetes') {
            steps { sh 'kubectl apply -f release/kubernetes-manifests.yaml' }
        }
    }
}
```

### Pipeline Results — Build #16 ✅

| Stage | Name | Time | Status |
|:---:|:---|:---:|:---:|
| 1 | Clone Code | 0.85s | 🟢 ✅ |
| 2 | Docker Build | 1s | 🟢 ✅ |
| 3 | Docker Push | 5s | 🟢 ✅ |
| 4 | Trivy Scan | 37s | 🩷 ✅ |
| 5 | K8s Deploy | 3s | 🟢 ✅ |
| — | **Total** | **51s** | **SUCCESS** |

<hr style="border:2px solid #72243E;border-radius:2px;">

## <span style="color:#72243E">🔒 Security Scanning with Trivy</span>

### Install Trivy

```bash
sudo apt install wget apt-transport-https gnupg -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb generic main | \
  sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt update && sudo apt install trivy -y
trivy --version
```

### Scan Results

| Target | Vulnerabilities |
|:---|:---:|
| 🩷 jhansi977/edustream:v1 (debian 13.4) | 17 found — non-blocking |
| 🟢 node-pkg: currencyservice | ✅ 0 CVEs — Clean |
| 🟢 node-pkg: paymentservice | ✅ 0 CVEs — Clean |

> [!NOTE]
> `--exit-code 0` means pipeline continues even when vulnerabilities found. Scan runs as visibility step only.

<hr style="border:2px solid #27500A;border-radius:2px;">

## <span style="color:#27500A">📊 Monitoring Setup</span>

### Step 1 — Install Helm + Prometheus Stack

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack
kubectl get pods | grep monitoring
```

### Step 2 — Expose Grafana and Prometheus

```bash
kubectl patch svc monitoring-grafana -p '{"spec": {"type": "NodePort"}}'
kubectl patch svc monitoring-kube-prometheus-prometheus -p '{"spec": {"type": "NodePort"}}'
kubectl get svc monitoring-grafana
```

### Step 3 — Fix Metrics Server for AWS

```bash
kubectl edit deployment metrics-server -n kube-system
# Add these args:
# - --kubelet-insecure-tls
# - --kubelet-preferred-address-types=InternalIP
# - --kubelet-use-node-status-port
kubectl rollout restart deployment metrics-server -n kube-system
kubectl top nodes
```

### Live Node Metrics ✅

```bash
kubectl top nodes
# NAME         CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
# k8s-master   184m         9%     2169Mi          28%
# k8s-worker   426m         21%    2547Mi          33%
```

### Prometheus Targets

| Target | Status |
|:---|:---:|
| 🟢 Grafana | ✅ UP |
| 🟢 Alertmanager | ✅ UP |
| 🟢 Kubernetes API Server | ✅ UP |
| 🟢 CoreDNS | ✅ UP |
| 🟢 Node Exporter | ✅ UP |

<hr style="border:2px solid #72243E;border-radius:2px;">

## <span style="color:#72243E">📈 Project Stats</span>

| 🟢 Metric | 🩷 Value |
|:---|:---:|
| AWS EC2 Instances | **3** |
| Microservices Deployed | **13** |
| Kubernetes Resources | **35** |
| Pipeline Stages | **5** |
| Total Build Time | **51 seconds** |
| Builds Completed | **16+** |
| Cluster Uptime | **18 days** |
| CVEs Blocking Deploy | **0** |

<hr style="border:2px solid #27500A;border-radius:2px;">

## <span style="color:#27500A">🔧 Troubleshooting Guide</span>

### Common Issues and Solutions

> [!CAUTION]
> Read each error carefully before applying the fix. Do not skip steps.

**1. CRI Runtime Error on kubeadm init**

```bash
# Error
[ERROR CRI]: container runtime is not running

# Fix
sudo apt remove -y containerd
sudo apt install -y containerd.io
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
# Set SystemdCgroup = true in config
sudo systemctl restart containerd
```

**2. Calico Pods Stuck at 0/1**

```bash
# Fix: Add All-Traffic inbound rule in AWS Security Group
# Source: same security group ID
kubectl delete pod -n kube-system -l k8s-app=calico-node
```

**3. DNS 500 Error Between Services**

```bash
kubectl delete pod -n kube-system -l k8s-app=kube-dns
kubectl exec -it <pod-name> -- nslookup currencyservice
```

**4. Jenkins Docker Permission Denied**

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

**5. Grafana Showing No Data**

```bash
kubectl edit deployment metrics-server -n kube-system
# Add: --kubelet-insecure-tls
# Add: --kubelet-preferred-address-types=InternalIP
kubectl rollout restart deployment metrics-server -n kube-system
kubectl top nodes
```

**6. Worker Node Not Joining**

```bash
kubeadm token create --print-join-command
sudo kubeadm join <MASTER-IP>:6443 \
  --token <new-token> \
  --discovery-token-ca-cert-hash sha256:<hash> \
  --cri-socket=unix:///var/run/cri-dockerd.sock
```

<hr style="border:2px solid #72243E;border-radius:2px;">

## <span style="color:#72243E">🤝 Contributing</span>

1. Fork the repository
2. Create a feature branch `git checkout -b feature/your-feature`
3. Commit your changes `git commit -m 'Add your feature'`
4. Push to the branch `git push origin feature/your-feature`
5. Open a Pull Request

```bash
git clone https://github.com/Jhansi-112/edustream-devsecops-platform.git
cd edustream-devsecops-platform
kubectl apply -f release/kubernetes-manifests.yaml
kubectl get pods
```

<hr style="border:2px solid #27500A;border-radius:2px;">

## <span style="color:#27500A">🛠️ Author & Community</span>

This project is maintained by **Jhansi** 💡

Your feedback and contributions are welcome!

- **GitHub:** [@Jhansi-112](https://github.com/Jhansi-112)
- **Live Portfolio:** [View Here](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html)

<hr style="border:2px solid #72243E;border-radius:2px;">

## <span style="color:#72243E">⭐ Support the Project</span>

If you found this project helpful, please consider:

- ⭐ Starring the repository
- 🔗 Sharing it with your network
- 🤝 Contributing to its improvement

![footer](https://capsule-render.vercel.app/api?type=waving&color=0:72243E,100:27500A&height=120&section=footer)

<div align="center">

*"The best way to learn DevOps is to break things and fix them."*

**— Jhansi 👩‍💻**

<br/>

<table>
<tr>
<td align="center">
<a href="https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html">
<div style="border:2px solid #27500A;border-radius:10px;overflow:hidden;display:inline-block;min-width:180px;">
<div style="background:#27500A;padding:5px 16px;text-align:center;">
<span style="color:#C0DD97;font-size:10px;font-weight:700;letter-spacing:2px;">✨ CLICK TO VIEW ✨</span>
</div>
<div style="padding:10px 16px;text-align:center;background:#f0faf0;">
<span style="color:#27500A;font-size:14px;font-weight:800;">🚀 LIVE PORTFOLIO</span>
</div>
</div>
</a>
</td>
<td width="20"></td>
<td align="center">
<a href="https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devsecops-architecture.html">
<div style="border:2px solid #72243E;border-radius:10px;overflow:hidden;display:inline-block;min-width:180px;">
<div style="background:#72243E;padding:5px 16px;text-align:center;">
<span style="color:#F4C0D1;font-size:10px;font-weight:700;letter-spacing:2px;">✨ CLICK TO VIEW ✨</span>
</div>
<div style="padding:10px 16px;text-align:center;background:#fff0f5;">
<span style="color:#72243E;font-size:14px;font-weight:800;">🏗️ ARCHITECTURE</span>
</div>
</div>
</a>
</td>
<td width="20"></td>
<td align="center">
<a href="https://github.com/Jhansi-112">
<img src="https://img.shields.io/badge/GitHub-Jhansi--112-181717?style=for-the-badge&logo=github"/>
</a>
</td>
</tr>
</table>

<br/>

**⭐ Star this repo if it helped you learn DevSecOps!**

</div>
