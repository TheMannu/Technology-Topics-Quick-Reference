# Why Monitoring is Required

### Key Reasons:
- **Automatic Log Deletion:** If the server goes down, logs may be automatically deleted, leading to loss of critical debugging information.
- **Manual Log Inspection:** In large environments, manually checking logs on multiple servers is inefficient and error-prone.
- **Smart Attackers:** Attackers often delete logs to cover their tracks, making timely monitoring critical.
- **Storage Limitations:** Keeping old logs on servers requires substantial storage, slowing down machines over time.

# Prometheus and Grafana for Monitoring

### **Prometheus**
Prometheus is an open-source toolkit for monitoring and alerting. It supports hardware and software monitoring, written in Go but supports multiple languages using client libraries.

#### **Key Components of Prometheus:**
1. **Data Retrieval Worker:**
   - Fetches data from target resources via HTTP endpoints.
   - Uses exporters to convert data to HTTP format if endpoints are unavailable.

2. **TSDB (Time-Series Database):**
   - Stores metrics collected by the data retrieval worker.
   - Makes data available to HTTP servers for user access.

3. **HTTP Server (Web/API Server):**
   - Displays metrics in a human-readable or graphical format for better understanding.

#### **Targeted Resources:**
- **Servers:** Linux/Windows for CPU, memory, and disk usage.
- **Services:** Exception tracking.
- **Applications and Web Servers:** Metrics like HTTP request counts or durations.

#### **Push Gateway:**
Used for short-lived jobs (e.g., updates, installations, backups). It collects metrics from these jobs and sends them to exporters when needed.

#### **Prometheus Configuration (YAML):**
Defines the components and parameters for data collection:
```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s
rule_files:
  - "rules.yml"
scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:8080"]
```

#### **Alert Manager:**
Prometheus sends alerts to an alert manager for further action:
- Notifications via email or other systems.

#### **Visualization:**
- **Default Web UI:** Built-in for PromQL queries.
- **Grafana:** Offers a user-friendly interface for visualizing metrics.



### **Grafana**
Grafana is an open-source visualization tool for metrics analysis. It helps simplify complex data using dashboards and supports features like:
- **Alerts:** Notifications based on metrics.
- **Collaboration:** Share dashboards with teams.
- **Time-Series Analytics:** Real-time insights.

#### **Integration with Prometheus:**
- Prometheus pulls and stores data from sources.
- Grafana reads this data and visualizes it for users.

---

# Helm for Kubernetes Monitoring

Helm is a Kubernetes package manager, similar to `apt` or `yum`, that simplifies application deployment using charts.

### Steps for Prometheus and Grafana Setup with Helm:
1. **Create a GKE Cluster:**
   Set up a Kubernetes cluster.

2. **Create a Monitoring Namespace:**
   ```bash
   kubectl create ns monitor
   ```

3. **Add Helm Repo:**
   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
   ```

4. **Install Prometheus and Grafana:**
   ```bash
   helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack --namespace monitor
   ```

5. **Verify Installation:**
   ```bash
   kubectl get pods -n monitor
   ```

6. **Expose Grafana Service:**
   ```bash
   kubectl expose deployment kube-prometheus-stack-grafana --port=3000 --target-port=3000 --name=grafana --type=LoadBalancer -n monitor
   ```

7. **Check Service and Login to Grafana:**
   ```bash
   kubectl get svc -n monitor
   ```
   Default login credentials: `admin/prom-operator`.

8. **Explore Dashboards:**
   Preconfigured dashboards available:
   - General / Kubernetes / Compute Resources / Cluster
   - General / Kubernetes / Compute Resources / Namespace (Pods)

9. **Delete Helm Chart (if needed):**
   ```bash
   helm uninstall [RELEASE_NAME]
   ```
