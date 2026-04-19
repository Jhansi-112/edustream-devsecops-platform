<div align="center">

# 🚀 EduStream DevSecOps Platform

[![Jenkins](https://img.shields.io/badge/Jenkins-CI/CD-D24939?style=for-the-badge&logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.29-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-jhansi977-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://hub.docker.com/)
[![Trivy](https://img.shields.io/badge/Trivy-Security-1904DA?style=for-the-badge&logo=aqua&logoColor=white)](https://trivy.dev/)
[![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-v12-F46800?style=for-the-badge&logo=grafana&logoColor=white)](https://grafana.com/)
[![AWS](https://img.shields.io/badge/AWS-EC2-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)](https://aws.amazon.com/)

<br/>

**🌐 Live Project Links**

[![Portfolio](https://img.shields.io/badge/👉%20View%20Full%20Portfolio-0a0d14?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html)
&nbsp;&nbsp;
[![Architecture](https://img.shields.io/badge/👉%20View%20Architecture-0a0d14?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devsecops-architecture.html)

</div>

---

## 📖 The Story — How This Project Was Born

It all started with a simple question:

> *"Can I build a real DevSecOps platform — completely from scratch — without using any managed services?"*

The answer became this project.

I started with **3 blank AWS EC2 instances**, no pre-built cluster, no shortcuts. Just Ubuntu, terminal, and determination. What followed was a journey of building, breaking, debugging, and learning — exactly the way real DevOps engineers do it in production.

This is not a tutorial follow-along. **Every command here was run by me. Every error was faced by me. Every fix was figured out by me.**

---

## 🗺️ The Journey — Phase by Phase

### 🏁 Chapter 1 — Setting Up the Foundation
*"Before anything runs, infrastructure must exist."*

I created **3 EC2 instances on AWS** — one for Jenkins, one as Kubernetes Master, one as Worker. Configured security groups, opened the right ports, and SSHed into each machine. The project had begun.

```
devops-server  →  Jenkins + Docker + kubectl
k8s-master     →  Kubernetes Control Plane
k8s-worker     →  Runs all 13 microservices
```

---

### ⚙️ Chapter 2 — Building the Kubernetes Cluster from Scratch
*"No EKS. No shortcuts. Just kubeadm and patience."*

I installed `kubeadm`, `kubelet`, and `kubectl` manually on both master and worker. Initialized the cluster, installed **Calico CNI** for pod networking, and joined the worker node.

But it was not smooth. I hit a real error:

```
[ERROR CRI]: container runtime is not running
unknown service runtime.v1.RuntimeService
```

After hours of debugging, the fix was **reinstalling containerd via Docker repo and using cri-dockerd**. The cluster came alive.

```bash
kubectl get nodes
# NAME         STATUS   ROLES           AGE
# k8s-master   Ready    control-plane   ✅
# k8s-worker   Ready    <none>          ✅
```

---

### 📦 Chapter 3 — Deploying 13 Microservices
*"One command. 13 services. 35 Kubernetes resources created."*

I deployed the full microservices application — frontend, cart, checkout, payment, shipping, email, recommendation, currency, ads, and redis — all running inside Kubernetes pods on the worker node.

```bash
kubectl apply -f release/kubernetes-manifests.yaml
# deployment.apps/frontend created
# deployment.apps/cartservice created
# deployment.apps/paymentservice created
# ... 35 resources total
```

Then another challenge — the app opened but showed a **500 DNS error**. Services could not talk to each other. The root cause? **Calico BGP was not established** between nodes because of a missing AWS Security Group rule. Fixed by allowing all traffic within the same security group.

---

### 🔁 Chapter 4 — Automating with Jenkins CI/CD
*"No more manual deployments. Every push triggers everything."*

I installed Jenkins on the DevOps server, connected it to GitHub via webhook, and wrote a 5-stage pipeline:

```
git push → Jenkins triggered → Docker build → Push to DockerHub
       → Trivy security scan → kubectl apply → Live on Kubernetes
```

But Jenkins could not access Docker. Error: `permission denied on /var/run/docker.sock`

Fix: `sudo usermod -aG docker jenkins`

Then Jenkins could not find the project folder. Fix: copy kubeconfig to Jenkins home and set correct file permissions.

After all fixes — the pipeline ran green. **Build #16. Total time: 51 seconds. All stages passed.**

---

### 🔒 Chapter 5 — Security Scanning with Trivy
*"Every image is scanned before it touches Kubernetes."*

Integrated **Trivy** directly into the Jenkins pipeline. Every build scans the Docker image for HIGH and CRITICAL vulnerabilities before deploying.

```bash
trivy image --exit-code 0 --severity HIGH,CRITICAL jhansi977/edustream:v1
```

| Target | Result |
|---|---|
| jhansi977/edustream:v1 (debian 13.4) | 17 found — non-blocking |
| node-pkg: currencyservice | ✅ 0 CVEs |
| node-pkg: paymentservice | ✅ 0 CVEs |

---

### 📊 Chapter 6 — Adding Eyes to the Cluster
*"A cluster you cannot see is a cluster you cannot trust."*

Installed **Prometheus** using Helm and connected **Grafana** for visualization. But Grafana showed **"No Data"** on all dashboards.

The problem? **Metrics Server was misconfigured.** Wrong arguments in the YAML caused `kubectl top` to fail completely.

Fixed by reinstalling metrics-server with the correct flags:
```bash
--kubelet-insecure-tls
--kubelet-preferred-address-types=InternalIP
```

After the fix:
```bash
kubectl top nodes
# k8s-master   184m   9%    2169Mi   28%
# k8s-worker   426m   21%   2547Mi   33%
```

Grafana dashboards lit up. CPU graphs, memory usage, pod health — all live. **18 days of continuous uptime.**

---

### 🏆 Chapter 7 — The Result
*"From 3 blank servers to a full DevSecOps platform."*

| What | Result |
|---|---|
| EC2 Nodes | 3 |
| Microservices Running | 13 |
| Pipeline Stages | 5 |
| Builds Completed | 16+ |
| Total Build Time | 51 seconds |
| K8s Resources | 35 |
| Cluster Uptime | 18 days |
| CVEs Blocking Deploy | 0 |

---

## 🧠 Lessons Learned — Real Errors, Real Fixes

| What Broke | Why | How I Fixed It |
|---|---|---|
| `CRI runtime not running` | containerd misconfigured | Reinstalled via Docker repo + cri-dockerd |
| Calico `0/1 Running` | BGP not established | Added All-Traffic SG rule (same SG → same SG) |
| Services DNS failing | Calico not ready | Fixed networking + restarted CoreDNS |
| Jenkins Docker denied | Jenkins not in Docker group | `sudo usermod -aG docker jenkins` |
| Grafana No Data | Metrics Server wrong config | Fixed args + `--kubelet-insecure-tls` |
| kubectl top failing | Wrong metrics-server YAML | Fixed deployment args + restarted |

---

## 🚀 Want to Build This Yourself?

Clone the repo and follow the steps:

```bash
git clone https://github.com/Jhansi-112/edustream-devsecops-platform.git
cd edustream-devsecops-platform
```

Then follow the phases above — Chapter by Chapter. Every command is real and tested. You will face errors. That is part of the journey. 💪

---

<div align="center">

*"The best way to learn DevOps is to break things and fix them."*

**— Jhansi 👩‍💻**

<br/>

[![Portfolio](https://img.shields.io/badge/🎯%20View%20Live%20Portfolio-0a0d14?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html)
&nbsp;
[![Architecture](https://img.shields.io/badge/🏗️%20View%20Architecture-0a0d14?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devsecops-architecture.html)
&nbsp;
[![GitHub](https://img.shields.io/badge/GitHub-Jhansi--112-181717?style=for-the-badge&logo=github)](https://github.com/Jhansi-112)

<br/>

**⭐ Star this repo if it helped you learn DevSecOps!**

</div>
