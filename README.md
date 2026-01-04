# 🚀 Kubernetes Monitoring Stack using Helm, KIND, Prometheus & Grafana

This project demonstrates how to set up a **complete Kubernetes monitoring stack** using **Helm charts** on a **KIND (Kubernetes IN Docker) cluster**, with **Prometheus** for metrics collection and **Grafana** for visualization.

---

## 📌 Tech Stack

- ☸️ Kubernetes (KIND)
- 📦 Helm
- 📊 Prometheus
- 📈 Grafana
- 🐳 Docker
- 🌐 NGINX Ingress (optional)

---

## 🧠 Architecture Overview

+-------------+ +----------------+
| Browser | ----> | Grafana |
+-------------+ +----------------+
|
v
+--------------+
| Prometheus |
+--------------+
|
v
+-------------------+
| Kubernetes Nodes |
| (Pods / Services)|
+-------------------+

---

## ⚙️ Prerequisites

Make sure the following tools are installed on your system:

- Docker
- KIND
- kubectl
- Helm

Verify:
```bash
docker --version
kind version
kubectl version --client
helm version
```

🚀 Step 1: Create KIND Cluster
kind create cluster --name monitoring-cluster --config kind-config.yaml

Verify:
kubectl get nodes

📦 Step 2: Add Helm Repositories

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

step 3: install

helm install prometheus-stack prometheus-community/kube-prometheus-stack \
  -n monitoring \
  --create-namespace \
  --set prometheus.service.type=NodePort \
  --set prometheus.service.nodePort=30000 \
  --set grafana.service.type=NodePort \
  --set grafana.service.nodePort=31000

verify:
kubectl get svc -n monitoring

Step4 Expose the ports:
kubectl port-forward svc/prometheus-stack-kube-prom-prometheus 9090:9090 -n monitorning --address=0.0.0.0 &
kubectl port-forward svc/prometheus-stack-grafana 3000:80 -n monitorning --address=0.0.0.0 &



🔐 Step 5: Get Grafana Admin Password

kubectl get secret grafana -n monitoring \
-o jsonpath="{.data.admin-password}" | base64 --decode


Import Dashboards

Recommended Dashboard IDs:

3119 → Kubernetes Cluster Monitoring

6417 → Kubernetes Pod Metrics

1860 → Node Exporter Full

