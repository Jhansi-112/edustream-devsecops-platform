<div align="center">

![Header](https://capsule-render.vercel.app/api?type=waving&color=0:0ea5e9,50:6366f1,100:00ff88&height=200&section=header&text=EduStream%20DevSecOps%20Platform&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Built%20from%20Scratch%20%7C%20AWS%20EC2%20%7C%20Kubernetes%20%7C%20Jenkins%20%7C%20Docker%20%7C%20Trivy%20%7C%20Prometheus%20%7C%20Grafana&descAlignY=58&descSize=14)

<br/>

[![Jenkins](https://img.shields.io/badge/Jenkins-CI/CD-D24939?style=for-the-badge&logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.29-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-jhansi977-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://hub.docker.com/)
[![Trivy](https://img.shields.io/badge/Trivy-Security-1904DA?style=for-the-badge&logo=aqua&logoColor=white)](https://trivy.dev/)
[![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-v12-F46800?style=for-the-badge&logo=grafana&logoColor=white)](https://grafana.com/)
[![AWS](https://img.shields.io/badge/AWS-EC2-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)](https://aws.amazon.com/)

<br/>

---

### 🌐 Live Project — Click to View

[![Portfolio](https://img.shields.io/badge/🎯%20VIEW%20FULL%20PORTFOLIO-6366f1?style=for-the-badge&logoColor=white)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html)
&nbsp;&nbsp;
[![Architecture](https://img.shields.io/badge/🏗️%20VIEW%20ARCHITECTURE-0ea5e9?style=for-the-badge&logoColor=white)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devsecops-architecture.html)

---

</div>

<br/>

# 📊 PROJECT STATS AT A GLANCE

| 🖥️ EC2 Nodes | 📦 Microservices | ⚙️ Stages | 🔨 Builds | ⚡ Build Time | ☸️ K8s Resources | ⏱️ Uptime | 🔒 CVEs |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **3** | **13** | **5** | **16+** | **51 sec** | **35** | **18 days** | **0** |

<br/>

---

# 🌟 WHAT IS THIS PROJECT?

> 💬 *"Every error in this project is real. Every fix is real. This is not a tutorial copy — built completely from scratch on AWS EC2."*

- ✅ Kubernetes cluster built **manually** using `kubeadm` — no EKS, no shortcuts
- ✅ Every `git push` **automatically** triggers Jenkins → Docker → Trivy → Kubernetes
- ✅ **Trivy security scanning** on every single build
- ✅ **Real-time monitoring** — Prometheus scrapes every 15 seconds → Grafana dashboards
- ✅ **18 days continuous uptime** — 13 microservices running without interruption

<br/>

---

# 🏗️ COMPLETE ARCHITECTURE FLOW

```
╔══════════════════════════════════════════════════════════════════════╗
║                    👩‍💻  DEVELOPER                                    ║
║                        git push                                     ║
╚══════════════════════════════╦═══════════════════════════════════════╝
                               ║ webhook trigger
                               ▼
╔══════════════════════════════════════════════════════════════════════╗
║          ⚙️  JENKINS CI/CD SERVER  ·  AWS EC2                       ║
║                                                                     ║
║  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  ║
║  │Stage  1 │→ │Stage  2 │→ │Stage  3 │→ │Stage  4 │→ │Stage  5 │  ║
║  │  Clone  │  │ Docker  │  │Push to  │  │  Trivy  │  │ kubectl │  ║
║  │  Repo   │  │  Build  │  │DockerHub│  │  Scan   │  │  apply  │  ║
║  │ 0.85s ✓ │  │  1s ✓  │  │  5s ✓  │  │ 37s ✓  │  │  3s ✓  │  ║
║  └─────────┘  └─────────┘  └─────────┘  └─────────┘  └─────────┘  ║
╚══════════════════════════════╦═══════════════════════════════════════╝
                               ║ docker push
                               ▼
                  ╔════════════════════════╗
                  ║   📦  DockerHub        ║
                  ║   jhansi977/           ║
                  ║   edustream:v1         ║
                  ╚════════════╦═══════════╝
                               ║ image pull (kubectl apply)
                               ▼
╔══════════════════════════════════════════════════════════════════════╗
║              ☸️  KUBERNETES CLUSTER  ·  AWS EC2                     ║
║                                                                     ║
║  ╔══════════════════════╗    ╔══════════════════════════════════╗   ║
║  ║    MASTER NODE       ║    ║         WORKER NODE             ║   ║
║  ║  ─────────────────   ║    ║  ──────────────────────────     ║   ║
║  ║  API Server          ║    ║  🌐 frontend       :30093       ║   ║
║  ║  Scheduler           ║───▶║  🛒 cartservice                 ║   ║
║  ║  etcd                ║    ║  💾 redis-cart                  ║   ║
║  ║  Controller Manager  ║    ║  💳 checkoutservice             ║   ║
║  ║  Calico CNI          ║    ║  💰 paymentservice              ║   ║
║  ║  Metrics Server      ║    ║  📦 shippingservice             ║   ║
║  ╚══════════════════════╝    ║  📧 emailservice                ║   ║
║                              ║  📋 productcatalogservice       ║   ║
║                              ║  💡 recommendationservice       ║   ║
║                              ║  💱 currencyservice             ║   ║
║                              ║  📢 adservice                   ║   ║
║                              ║  🔄 loadgenerator               ║   ║
║                              ╚══════════════════════════════════╝   ║
╚══════════════════════════════╦═══════════════════════════════════════╝
                               ║ NodePort :30093
                               ▼
                        👥 USER BROWSER
                               ║
                               ▼
╔══════════════════════════════════════════════════════════════════════╗
║                  📊  MONITORING STACK                               ║
║                                                                     ║
║   Metrics Server ──┐                                                ║
║   Node Exporter  ──┼── scrape 15s ──▶ Prometheus ──▶ Grafana       ║
║   Pod /metrics   ──┘    port 9090        port 3000                  ║
║                         CPU · Memory · Pod Health Dashboards        ║
╚══════════════════════════════════════════════════════════════════════╝
```

<br/>

---

# 🗺️ PROJECT PHASES

## 🟣 PHASE 1 — AWS Infrastructure Setup

> *"Before anything runs, infrastructure must exist."*

Created **3 EC2 instances on AWS** — Jenkins server, Kubernetes Master, and Worker node. Configured all security groups and networking.

```
devops-server  →  Jenkins + Docker + kubectl  →  Ubuntu 22.04
k8s-master     →  Kubernetes Control Plane    →  Ubuntu 22.04
k8s-worker     →  Runs all 13 microservices   →  Ubuntu 22.04
```

**Ports configured:**

| Port | Service |
|---|---|
| 22 | SSH |
| 80 | HTTP |
| 8080 | Jenkins UI |
| 6443 | Kubernetes API |
| 3000 | Grafana |
| 9090 | Prometheus |
| 30000–32767 | Kubernetes NodePorts |

<br/>

## 🔵 PHASE 2 — Kubernetes Cluster from Scratch

> *"No EKS. No shortcuts. Just kubeadm and patience."*

Installed `kubeadm`, `kubelet`, `kubectl` manually. Hit a real error:

```
❌ [ERROR CRI]: container runtime is not running
   unknown service runtime.v1.RuntimeService
```

**Fix:** Reinstalled containerd via Docker repo + used cri-dockerd

```bash
# After fix — cluster came alive ✅
kubectl get nodes
# NAME         STATUS   ROLES           VERSION
# k8s-master   Ready    control-plane   v1.29.15
# k8s-worker   Ready    <none>          v1.29.15
```

<br/>

## 🟢 PHASE 3 — Deploy 13 Microservices

> *"One command. 13 services. 35 Kubernetes resources created."*

```bash
kubectl apply -f release/kubernetes-manifests.yaml
# ✅ frontend created
# ✅ cartservice created
# ✅ paymentservice created
# ✅ checkoutservice created
# ✅ ... 35 resources total
```

Hit DNS error — services could not talk to each other. Root cause: **Calico BGP not established** due to missing AWS Security Group rule. Fixed by allowing all traffic within same security group.

<br/>

## 🟡 PHASE 4 — Jenkins CI/CD Pipeline

> *"No more manual deployments. Every push triggers everything."*

**Pipeline flow:**
```
git push → webhook → Jenkins → Docker build → DockerHub → Trivy → Kubernetes
```

**Errors fixed during setup:**

| Error | Fix |
|---|---|
| Docker permission denied | `sudo usermod -aG docker jenkins` |
| kubeconfig not found | Copied config to `/var/lib/jenkins/.kube/` |
| File permission issues | `sudo chown -R jenkins:jenkins` |

**Final build result:**
```
✅ Stage 1  Clone Code      0.85s
✅ Stage 2  Docker Build    1s
✅ Stage 3  Docker Push     5s
✅ Stage 4  Trivy Scan      37s
✅ Stage 5  K8s Deploy      3s
─────────────────────────────────
   Build #16  ·  51s  ·  SUCCESS
```

<br/>

## 🔴 PHASE 5 — Trivy Security Scanning

> *"Every image is scanned before it touches Kubernetes."*

```bash
trivy image --exit-code 0 --severity HIGH,CRITICAL \
  --format table jhansi977/edustream:v1
```

| Target | Result |
|---|---|
| jhansi977/edustream:v1 (debian 13.4) | 17 found — non-blocking |
| node-pkg: currencyservice | ✅ 0 CVEs — Clean |
| node-pkg: paymentservice | ✅ 0 CVEs — Clean |

> `--exit-code 0` — pipeline does not block. Scan runs as visibility step only ✅

<br/>

## 🟠 PHASE 6 — Prometheus + Grafana Monitoring

> *"A cluster you cannot see is a cluster you cannot trust."*

Grafana showed **"No Data"** — Metrics Server was misconfigured.

**Fix:**
```bash
# Added these flags to metrics-server deployment
--kubelet-insecure-tls
--kubelet-preferred-address-types=InternalIP
```

**After fix — live metrics:**
```bash
kubectl top nodes
# NAME         CPU    CPU%   MEMORY    MEMORY%
# k8s-master   184m   9%     2169Mi    28%
# k8s-worker   426m   21%    2547Mi    33%
```

**Prometheus Targets — All UP ✅**

| Target | Status |
|---|---|
| Grafana | ✅ UP |
| Alertmanager | ✅ UP |
| Kubernetes API Server | ✅ UP |
| CoreDNS | ✅ UP |
| Node Exporter | ✅ UP |

<br/>

## 🏆 PHASE 7 — Final Result

> *"From 3 blank servers to a full DevSecOps platform."*

```
╔══════════════════════════════════════════════╗
║           FINAL PROJECT STATS               ║
╠══════════════════╦═══════════════════════════╣
║  EC2 Nodes       ║  3                        ║
║  Microservices   ║  13                       ║
║  Pipeline Stages ║  5                        ║
║  Builds Run      ║  16+                      ║
║  Build Time      ║  51 seconds               ║
║  K8s Resources   ║  35                       ║
║  Cluster Uptime  ║  18 days                  ║
║  CVEs Blocking   ║  0                        ║
╚══════════════════╩═══════════════════════════╝
```

<br/>

---

# 🧠 REAL PROBLEMS I DEBUGGED & FIXED

| ❌ Problem | 🔍 Root Cause | ✅ Fix Applied |
|---|---|---|
| `CRI runtime not running` | containerd misconfigured | Reinstalled via Docker repo + cri-dockerd |
| Calico pods `0/1 Running` | BGP not established | Added All-Traffic SG rule (same SG → same SG) |
| DNS failure between services | Calico not ready → CoreDNS failing | Fixed networking + restarted CoreDNS pods |
| Jenkins `permission denied` | Jenkins not in Docker group | `sudo usermod -aG docker jenkins` |
| Grafana `No Data` | Metrics Server wrong config | Reinstalled + `--kubelet-insecure-tls` |
| `kubectl top` not working | Wrong metrics-server YAML args | Fixed deployment args + restarted |

<br/>

---

# 🚀 PRACTICE THIS PROJECT YOURSELF

```bash
# Step 1 — Clone this repo
git clone https://github.com/Jhansi-112/edustream-devsecops-platform.git
cd edustream-devsecops-platform
```

Follow the **7 phases above** — step by step. Every command is real and tested.

> 💡 *You will face errors. That is part of the journey. Fix them and you will learn more than any course can teach.*

<br/>

---

![Footer](https://capsule-render.vercel.app/api?type=waving&color=0:00ff88,50:6366f1,100:0ea5e9&height=120&section=footer)

<div align="center">

**Built from scratch by Jhansi 👩‍💻**

*Real project · Real errors · Real fixes · Real learning*

<br/>

[![Portfolio](https://img.shields.io/badge/🎯%20View%20Live%20Portfolio-6366f1?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html)
&nbsp;
[![Architecture](https://img.shields.io/badge/🏗️%20View%20Architecture-0ea5e9?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devsecops-architecture.html)
&nbsp;
[![GitHub](https://img.shields.io/badge/GitHub-Jhansi--112-181717?style=for-the-badge&logo=github)](https://github.com/Jhansi-112)

<br/>

**⭐ Star this repo if it helped you learn DevSecOps!**

</div>
