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
