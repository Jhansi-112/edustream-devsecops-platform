<div align="center">

![header](https://capsule-render.vercel.app/api?type=waving&color=0:6366f1,100:06b6d4&height=200&section=header&text=EduStream%20DevSecOps%20Platform&fontSize=38&fontColor=ffffff&fontAlignY=38&desc=A%20real%20project.%20Real%20errors.%20Real%20fixes.%20Built%20from%20scratch%20on%20AWS.&descSize=14&descAlignY=58&descColor=c7d2fe)

<br/>

[![Jenkins](https://img.shields.io/badge/Jenkins-6366f1?style=for-the-badge&logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-06b6d4?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-6366f1?style=for-the-badge&logo=docker&logoColor=white)](https://hub.docker.com/)
[![Trivy](https://img.shields.io/badge/Trivy-06b6d4?style=for-the-badge&logo=aqua&logoColor=white)](https://trivy.dev/)
[![Prometheus](https://img.shields.io/badge/Prometheus-6366f1?style=for-the-badge&logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-06b6d4?style=for-the-badge&logo=grafana&logoColor=white)](https://grafana.com/)
[![AWS](https://img.shields.io/badge/AWS%20EC2-6366f1?style=for-the-badge&logo=amazonaws&logoColor=white)](https://aws.amazon.com/)

<br/>

[![Portfolio](https://img.shields.io/badge/🎯%20View%20Live%20Portfolio-6366f1?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html)
&nbsp;&nbsp;
[![Architecture](https://img.shields.io/badge/🏗️%20View%20Architecture%20Diagram-06b6d4?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devsecops-architecture.html)

</div>

---

## 👋 About This Project

I built this project to answer one question I kept asking myself:

> *Can I deploy a real microservices application on Kubernetes — without using any cloud-managed service — completely from scratch?*

So I took 3 blank AWS EC2 servers, opened a terminal, and started building.

No EKS. No managed databases. No shortcuts.

What came out was a full DevSecOps pipeline — infrastructure, containers, CI/CD, security scanning, and monitoring — all working together. This README documents exactly what I built, what broke, and how I fixed it.

---

## 🗂️ What's Inside This Repo

```
edustream-devsecops-platform/
│
├── Dockerfile                       # Container image definition
├── Jenkinsfile                      # 5-stage CI/CD pipeline
├── release/
│   └── kubernetes-manifests.yaml   # All 13 microservice deployments
├── edustream-devops-portfolio.html  # Live project portfolio
└── edustream-devsecops-architecture.html  # Architecture diagram
```

---

## 🏗️ Architecture

Here is how every piece connects — from a developer pushing code to the app going live and being monitored:

```
  Developer
     │
     │  git push
     ▼
  ┌──────────────────┐
  │    GitHub Repo   │──── webhook fires ────────────────────────┐
  └──────────────────┘                                           │
                                                                 ▼
                                            ┌────────────────────────────────┐
                                            │     Jenkins  ·  EC2 Server     │
                                            │                                │
                                            │  Stage 1 ── Clone Repo  0.85s  │
                                            │      │                         │
                                            │  Stage 2 ── Docker Build  1s   │
                                            │      │                         │
                                            │  Stage 3 ── Push DockerHub  5s │
                                            │      │                         │
                                            │  Stage 4 ── Trivy Scan  37s    │
                                            │      │                         │
                                            │  Stage 5 ── kubectl apply  3s  │
                                            └────────────────┬───────────────┘
                                                             │
                                   ┌─────────────────────────┘
                                   │
                    ┌──────────────▼──────────────┐
                    │         DockerHub            │
                    │    jhansi977/edustream:v1    │
                    └──────────────┬──────────────┘
                                   │  image pull
                                   ▼
          ┌────────────────────────────────────────────────────┐
          │           Kubernetes Cluster  ·  AWS EC2           │
          │                                                    │
          │   ┌─────────────────┐     ┌──────────────────────┐ │
          │   │   Master Node   │     │     Worker Node      │ │
          │   │                 │     │                      │ │
          │   │  API Server     │────▶│  frontend  :30093    │ │
          │   │  Scheduler      │     │  cartservice         │ │
          │   │  etcd           │     │  redis-cart          │ │
          │   │  Controller Mgr │     │  checkoutservice     │ │
          │   │  Calico CNI     │     │  paymentservice      │ │
          │   │  Metrics Server │     │  shippingservice     │ │
          │   └─────────────────┘     │  emailservice        │ │
          │                          │  productcatalog      │ │
          │                          │  recommendation      │ │
          │                          │  currencyservice     │ │
          │                          │  adservice           │ │
          │                          │  loadgenerator       │ │
          │                          └──────────────────────┘ │
          └────────────────────────────────┬───────────────────┘
                                           │  NodePort :30093
                                           ▼
                                      User Browser
                                           │
                    ┌──────────────────────┘
                    │
                    ▼
          ┌─────────────────────────────────────────┐
          │           Monitoring Stack              │
          │                                        │
          │  ┌──────────────┐   ┌───────────────┐  │
          │  │  Prometheus  │──▶│    Grafana    │  │
          │  │  port 9090   │   │   port 3000   │  │
          │  └──────┬───────┘   └───────────────┘  │
          │         │                              │
          │  scrapes every 15 seconds from:        │
          │  · Metrics Server (kubectl top)        │
          │  · Node Exporter  (CPU / memory)       │
          │  · Pod /metrics   (Go runtime)         │
          └─────────────────────────────────────────┘
```

---

## 📊 Numbers That Matter

| Metric | Value |
|---|---|
| AWS EC2 Instances | 3 |
| Microservices Deployed | 13 |
| Kubernetes Resources Created | 35 |
| Jenkins Pipeline Stages | 5 |
| Total Build Time | 51 seconds |
| Builds Completed | 16+ |
| Cluster Uptime | 18 days |
| CVEs Blocking Deployment | 0 |

---

## 📖 How I Built It — The Full Story

---

### Chapter 1 — Infrastructure

> *"Before anything runs, infrastructure must exist."*

I created 3 EC2 instances on AWS — all Ubuntu 22.04. One for Jenkins and DevOps tooling, one as the Kubernetes master, one as the worker where all pods run.

The first thing I did was configure the security group properly — because later I learned that missing a single port rule can bring down the entire cluster networking.

```
devops-server  →  Jenkins · Docker · kubectl
k8s-master     →  API Server · Scheduler · etcd · Controller · Calico CNI
k8s-worker     →  Runs all 13 application pods
```

Ports I opened:

| Port | Why |
|---|---|
| 22 | SSH into servers |
| 8080 | Jenkins UI |
| 6443 | Kubernetes API |
| 3000 | Grafana |
| 9090 | Prometheus |
| 30000–32767 | Kubernetes NodePorts |

---

### Chapter 2 — Kubernetes Cluster

> *"No managed Kubernetes. Just kubeadm, three servers, and a lot of patience."*

I installed `kubeadm`, `kubelet`, and `kubectl` manually on both master and worker. Then ran `kubeadm init` — and immediately hit this:

```
[ERROR CRI]: container runtime is not running
unknown service runtime.v1.RuntimeService
```

This error stopped me cold. The container runtime was not talking to Kubernetes. After debugging, I found that the default Ubuntu `containerd` package ships with the CRI plugin disabled.

**Fix:** Uninstalled the broken containerd, reinstalled from Docker's official repository, and used `cri-dockerd` as the shim. Configured `SystemdCgroup = true` in the containerd config.

After that — the cluster initialized cleanly:

```bash
kubectl get nodes
# NAME         STATUS   ROLES           VERSION
# k8s-master   Ready    control-plane   v1.29.15
# k8s-worker   Ready    <none>          v1.29.15
```

Installed **Calico CNI** for pod-to-pod networking. This matters — without a network plugin, pods cannot communicate.

---

### Chapter 3 — Deploying the Application

> *"One command. Thirteen services. Thirty-five Kubernetes resources."*

```bash
kubectl apply -f release/kubernetes-manifests.yaml
```

All pods came up. I opened the browser, hit the NodePort — and got a **500 Internal Server Error**.

```
rpc error: code = Unavailable
desc = dns: A record lookup error: lookup currencyservice
dial udp 10.96.0.10:53: i/o timeout
```

The frontend could not resolve `currencyservice`. DNS was broken. The root cause was that **Calico's BGP peering had not established** between master and worker — because I had not allowed internal node-to-node traffic in the AWS security group.

**Fix:** Added an inbound rule allowing **All Traffic** from the same security group. Restarted Calico pods. DNS resolved. Services started talking. Application opened.

---

### Chapter 4 — Jenkins CI/CD Pipeline

> *"Manual deployment is a liability. Automation is the answer."*

Installed Jenkins on the devops server and wrote a 5-stage pipeline:

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

Three errors before it worked:

**Error 1** — Docker permission denied  
Jenkins runs as its own user and cannot access Docker socket by default.  
Fix: `sudo usermod -aG docker jenkins`

**Error 2** — kubectl cannot connect to cluster  
Jenkins did not have the kubeconfig file.  
Fix: Copied `/etc/kubernetes/admin.conf` to `/var/lib/jenkins/.kube/config`

**Error 3** — File permission denied on project folder  
Fix: `sudo chown -R jenkins:jenkins /home/ubuntu/edustream-devsecops-platform`

After all three fixes — Build #16 ran clean:

```
Stage 1  Clone Code      ✅  0.85s
Stage 2  Docker Build    ✅  1s
Stage 3  Docker Push     ✅  5s
Stage 4  Trivy Scan      ✅  37s
Stage 5  K8s Deploy      ✅  3s
─────────────────────────────────
Total time: 51 seconds · SUCCESS
```

---

### Chapter 5 — Security Scanning

> *"You cannot ship what you have not checked."*

Trivy scans the Docker image on every single build — before it ever reaches Kubernetes.

```bash
trivy image --exit-code 0 --severity HIGH,CRITICAL \
  --format table jhansi977/edustream:v1
```

| What Was Scanned | Result |
|---|---|
| Base image — debian 13.4 | 17 vulnerabilities (OS level) |
| currencyservice (Node.js packages) | ✅ Clean |
| paymentservice (Node.js packages) | ✅ Clean |

The `--exit-code 0` flag means the pipeline does not fail even if vulnerabilities are found. This is intentional — the scan runs as a reporting and visibility step. The results are logged in Jenkins for every build.

---

### Chapter 6 — Monitoring

> *"If you are not measuring it, you are not managing it."*

Installed the full **kube-prometheus-stack** using Helm — which includes Prometheus, Grafana, Alertmanager, and Node Exporter in one deployment.

But Grafana showed **"No Data"** on every dashboard.

I opened Prometheus targets — most were UP, but `kubectl top nodes` returned:
```
error: Metrics API not available
```

The Metrics Server was installed but not working. The problem was two wrong arguments in its deployment YAML that prevented it from connecting to the kubelet on AWS EC2.

**Fix:**
```yaml
args:
  - --kubelet-insecure-tls
  - --kubelet-preferred-address-types=InternalIP
  - --kubelet-use-node-status-port
```

After restarting the deployment:

```bash
kubectl top nodes
NAME         CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
k8s-master   184m         9%     2169Mi          28%
k8s-worker   426m         21%    2547Mi          33%
```

Grafana dashboards came alive. CPU usage, memory consumption, pod-level metrics — all visible in real time. The cluster had eyes.

---

### Chapter 7 — Final State

> *"From 3 blank EC2 servers to a running DevSecOps platform — 18 days of continuous uptime."*

Thirteen microservices. One automated pipeline. Security scanning on every build. Real-time monitoring. Everything connected and working.

---

## 🧠 Problems I Debugged — In Plain Language

These were not small issues. Each one took real debugging time.

| Problem | What Was Actually Wrong | How I Fixed It |
|---|---|---|
| CRI runtime error on kubeadm init | Ubuntu's containerd ships with CRI plugin disabled | Reinstalled from Docker repo, used cri-dockerd |
| Calico pods stuck at 0/1 | BGP could not establish because node-to-node traffic was blocked by AWS SG | Added All-Traffic inbound rule for same security group |
| App showing DNS 500 error | CoreDNS could not resolve service names because Calico was not ready | Fixed SG rule, restarted Calico and CoreDNS |
| Jenkins Docker permission denied | Jenkins user has no access to Docker socket | Added Jenkins to Docker group |
| Grafana showing No Data | Metrics Server could not reach kubelet on AWS | Fixed deployment args, added insecure-tls flag |
| kubectl top not working | Wrong kubelet address type for EC2 | Set InternalIP as preferred address type |

---

## 🚀 Try It Yourself

If you want to build the same project, clone this repo and follow each chapter above.

```bash
git clone https://github.com/Jhansi-112/edustream-devsecops-platform.git
cd edustream-devsecops-platform
```

Every command in this README is real. You will hit errors — probably the same ones I hit. That is fine. Read the error message carefully, understand what it is saying, and fix it. That is how this project was built.

> *The goal is not to copy commands. The goal is to understand what each command does and why.*

---

![footer](https://capsule-render.vercel.app/api?type=waving&color=0:06b6d4,100:6366f1&height=120&section=footer)

<div align="center">

**Built by Jhansi 👩‍💻**

<br/>

[![Portfolio](https://img.shields.io/badge/🎯%20View%20Live%20Portfolio-6366f1?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html)
&nbsp;
[![Architecture](https://img.shields.io/badge/🏗️%20View%20Architecture-06b6d4?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devsecops-architecture.html)
&nbsp;
[![GitHub](https://img.shields.io/badge/GitHub-Jhansi--112-181717?style=for-the-badge&logo=github)](https://github.com/Jhansi-112)

<br/>

**⭐ If this helped you — give it a star!**

</div>
