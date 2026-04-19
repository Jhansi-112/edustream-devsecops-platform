<div align="center">

![header](https://capsule-render.vercel.app/api?type=waving&color=0:6366f1,100:06b6d4&height=220&section=header&text=EduStream%20DevSecOps%20Platform&fontSize=40&fontColor=ffffff&fontAlignY=40&desc=Built%20from%20Scratch%20%7C%20AWS%20%7C%20Kubernetes%20%7C%20Jenkins%20%7C%20Docker%20%7C%20Trivy%20%7C%20Prometheus%20%7C%20Grafana&descSize=13&descAlignY=60&descColor=c7d2fe)

<br/>

[![Jenkins](https://img.shields.io/badge/Jenkins-6366f1?style=for-the-badge&logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-06b6d4?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-6366f1?style=for-the-badge&logo=docker&logoColor=white)](https://hub.docker.com/)
[![Trivy](https://img.shields.io/badge/Trivy-06b6d4?style=for-the-badge&logo=aqua&logoColor=white)](https://trivy.dev/)
[![Prometheus](https://img.shields.io/badge/Prometheus-6366f1?style=for-the-badge&logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-06b6d4?style=for-the-badge&logo=grafana&logoColor=white)](https://grafana.com/)
[![AWS](https://img.shields.io/badge/AWS-6366f1?style=for-the-badge&logo=amazonaws&logoColor=white)](https://aws.amazon.com/)

<br/>

[![Portfolio](https://img.shields.io/badge/🎯%20VIEW%20LIVE%20PORTFOLIO-6366f1?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html)
&nbsp;&nbsp;
[![Architecture](https://img.shields.io/badge/🏗️%20VIEW%20ARCHITECTURE-06b6d4?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devsecops-architecture.html)

<br/>

> ### *"Can I build a real DevSecOps platform — completely from scratch — without using any managed services?"*
> #### *The answer became this project.*

</div>

---

<div align="center">

### 📊 AT A GLANCE

| 🖥️ Nodes | 📦 Services | ⚙️ Stages | 🔨 Builds | ⚡ Time | ☸️ Resources | ⏱️ Uptime | 🔒 CVEs |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **3** | **13** | **5** | **16+** | **51s** | **35** | **18 days** | **0** |

</div>

---

## 🏗️ Architecture — How Everything Connects

```
  ╭─────────────────────────────────────────────────────────────────╮
  │                                                                 │
  │   👩‍💻  You write code and push to GitHub                        │
  │                        │                                        │
  │                        │ webhook fires instantly                │
  │                        ▼                                        │
  │   ┌─────────────────────────────────────────────────────────┐   │
  │   │            ⚙️  Jenkins  ·  AWS EC2                      │   │
  │   │                                                         │   │
  │   │   Clone  ──▶  Build  ──▶  Push  ──▶  Scan  ──▶  Deploy │   │
  │   │   0.85s       1s        5s       37s       3s           │   │
  │   └─────────────────────────┬───────────────────────────────┘   │
  │                             │                                   │
  │                             ▼                                   │
  │                   ┌──────────────────┐                          │
  │                   │  📦  DockerHub   │                          │
  │                   │  jhansi977/      │                          │
  │                   │  edustream:v1    │                          │
  │                   └────────┬─────────┘                          │
  │                            │ image pull                         │
  │                            ▼                                    │
  │   ┌─────────────────────────────────────────────────────────┐   │
  │   │           ☸️  Kubernetes  ·  AWS EC2                    │   │
  │   │                                                         │   │
  │   │   ┌──────────────┐      ┌──────────────────────────┐   │   │
  │   │   │ Master Node  │      │      Worker Node          │   │   │
  │   │   │              │      │                           │   │   │
  │   │   │ API Server   │─────▶│  frontend     :30093      │   │   │
  │   │   │ Scheduler    │      │  cartservice              │   │   │
  │   │   │ etcd         │      │  redis-cart               │   │   │
  │   │   │ Controller   │      │  checkoutservice          │   │   │
  │   │   │ Calico CNI   │      │  paymentservice           │   │   │
  │   │   │ Metrics Svr  │      │  shippingservice          │   │   │
  │   │   └──────────────┘      │  emailservice             │   │   │
  │   │                         │  productcatalog           │   │   │
  │   │                         │  recommendation           │   │   │
  │   │                         │  currencyservice          │   │   │
  │   │                         │  adservice                │   │   │
  │   │                         │  loadgenerator            │   │   │
  │   │                         └──────────────────────────┘   │   │
  │   └─────────────────────────────┬───────────────────────────┘   │
  │                                 │ NodePort :30093               │
  │                                 ▼                               │
  │                          👥 User Browser                        │
  │                                 │                               │
  │                                 ▼                               │
  │   ┌─────────────────────────────────────────────────────────┐   │
  │   │          📊  Monitoring Stack                           │   │
  │   │                                                         │   │
  │   │  Metrics Server ─┐                                      │   │
  │   │  Node Exporter  ─┼── 15s scrape ──▶ Prometheus ──▶ Grafana  │
  │   │  Pod /metrics   ─┘    port 9090       port 3000         │   │
  │   └─────────────────────────────────────────────────────────┘   │
  │                                                                 │
  ╰─────────────────────────────────────────────────────────────────╯
```

---

## 📖 The Story — Chapter by Chapter

---

### 🏁 Chapter 1 — Setting Up the Foundation

> *"Before anything runs, infrastructure must exist."*

I started with **3 blank AWS EC2 instances** — no pre-built cluster, no shortcuts. Just Ubuntu, a terminal, and determination.

```
devops-server  →  Jenkins + Docker + kubectl
k8s-master     →  Kubernetes Control Plane
k8s-worker     →  Runs all 13 microservices
```

Configured security groups, opened ports, SSHed into each machine. The journey had begun.

---

### ⚙️ Chapter 2 — Kubernetes Cluster from Scratch

> *"No EKS. No shortcuts. Just kubeadm and patience."*

Installed `kubeadm`, `kubelet`, `kubectl` manually. Hit a real wall immediately:

```
❌  [ERROR CRI]: container runtime is not running
    unknown service runtime.v1.RuntimeService
```

Hours of debugging. The fix — **reinstalling containerd via Docker repo + cri-dockerd**. The cluster finally came alive:

```bash
kubectl get nodes
# k8s-master   Ready    control-plane   v1.29.15  ✅
# k8s-worker   Ready    <none>          v1.29.15  ✅
```

---

### 📦 Chapter 3 — Deploying 13 Microservices

> *"One command. 13 services. 35 Kubernetes resources created."*

```bash
kubectl apply -f release/kubernetes-manifests.yaml
# frontend, cartservice, paymentservice, checkoutservice...
# 35 resources — all created ✅
```

Then — the app opened but showed a **500 DNS error**. Services could not talk to each other. Root cause: **Calico BGP was not established** between nodes due to a missing AWS Security Group rule. Fixed by allowing all traffic within the same security group.

Application live at → `http://<WORKER-IP>:30093` ✅

---

### 🔁 Chapter 4 — Jenkins CI/CD Automation

> *"No more manual deployments. Every push triggers everything."*

```
git push → webhook → Jenkins → Docker Build → DockerHub → Trivy Scan → Kubernetes
```

Real errors faced and fixed:

| Error | Fix |
|---|---|
| Docker `permission denied` | `sudo usermod -aG docker jenkins` |
| kubeconfig not found | Copied to `/var/lib/jenkins/.kube/` |
| File permission issues | `sudo chown -R jenkins:jenkins` |

Pipeline result after all fixes:

```
✅  Clone Code     0.85s
✅  Docker Build   1s
✅  Docker Push    5s
✅  Trivy Scan     37s
✅  K8s Deploy     3s
──────────────────────
    Build #16  ·  51s  ·  SUCCESS
```

---

### 🔒 Chapter 5 — Trivy Security Scanning

> *"Every image is scanned before it touches Kubernetes."*

```bash
trivy image --exit-code 0 --severity HIGH,CRITICAL jhansi977/edustream:v1
```

| Target | Result |
|---|---|
| edustream:v1 (debian 13.4) | 17 found — non-blocking |
| node-pkg: currencyservice | ✅ 0 CVEs — Clean |
| node-pkg: paymentservice | ✅ 0 CVEs — Clean |

> `--exit-code 0` — pipeline does not stop on vulnerabilities. Runs as a visibility step only.

---

### 📊 Chapter 6 — Prometheus + Grafana Monitoring

> *"A cluster you cannot see is a cluster you cannot trust."*

Grafana showed **"No Data"** — Metrics Server was misconfigured with wrong YAML args. Fixed:

```bash
# Added these flags to metrics-server deployment
--kubelet-insecure-tls
--kubelet-preferred-address-types=InternalIP
```

After the fix — everything lit up:

```bash
kubectl top nodes
# k8s-master   184m   9%    2169Mi   28%
# k8s-worker   426m   21%   2547Mi   33%
```

Prometheus targets — all UP ✅ | Grafana dashboards — live ✅ | **18 days uptime** 🎉

---

### 🏆 Chapter 7 — The Final Result

> *"From 3 blank servers to a full DevSecOps platform."*

| What | Result |
|---|---|
| EC2 Nodes | 3 |
| Microservices Running | 13 |
| Pipeline Stages | 5 |
| Builds Completed | 16+ |
| Total Build Time | 51 seconds |
| K8s Resources Applied | 35 |
| Cluster Uptime | 18 days |
| CVEs Blocking Deploy | 0 |

---

## 🧠 Real Errors — Real Fixes

| ❌ What Broke | 🔍 Why | ✅ How I Fixed It |
|---|---|---|
| `CRI runtime not running` | containerd misconfigured | Reinstalled via Docker repo + cri-dockerd |
| Calico `0/1 Running` | BGP not established | Added All-Traffic rule (same SG → same SG) |
| Services DNS failing | Calico not ready → CoreDNS down | Fixed networking + restarted CoreDNS |
| Jenkins Docker denied | Jenkins not in Docker group | `sudo usermod -aG docker jenkins` |
| Grafana `No Data` | Metrics Server wrong config | Reinstalled + `--kubelet-insecure-tls` |
| `kubectl top` failing | Wrong metrics-server YAML args | Fixed args + restarted deployment |

---

## 🚀 Want to Build This Yourself?

```bash
git clone https://github.com/Jhansi-112/edustream-devsecops-platform.git
cd edustream-devsecops-platform
```

Follow the chapters above — step by step. Every command is real and tested.

> 💡 *You will face errors. That is part of the journey. Fix them and you will learn more than any course can teach.* 💪

---

![footer](https://capsule-render.vercel.app/api?type=waving&color=0:06b6d4,100:6366f1&height=120&section=footer)

<div align="center">

*"The best way to learn DevOps is to break things and fix them."*

**— Jhansi 👩‍💻**

<br/>

[![Portfolio](https://img.shields.io/badge/🎯%20View%20Live%20Portfolio-6366f1?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devops-portfolio.html)
&nbsp;
[![Architecture](https://img.shields.io/badge/🏗️%20View%20Architecture-06b6d4?style=for-the-badge)](https://jhansi-112.github.io/edustream-devsecops-platform/edustream-devsecops-architecture.html)
&nbsp;
[![GitHub](https://img.shields.io/badge/GitHub-Jhansi--112-181717?style=for-the-badge&logo=github)](https://github.com/Jhansi-112)

<br/>

**⭐ Star this repo if it helped you learn DevSecOps!**

</div>
