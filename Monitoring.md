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
