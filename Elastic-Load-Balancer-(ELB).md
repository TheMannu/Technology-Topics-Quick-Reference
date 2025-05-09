# **Elastic Load Balancer (ELB) - Comprehensive Guide**

## **1. Introduction to Load Balancers**
A **Load Balancer** acts as a **traffic manager** for web applications. It **evenly distributes** incoming web traffic across multiple servers (instances), ensuring that no single server gets overwhelmed. This improves **performance, reliability, and fault tolerance** while maintaining high availability.

### **Key Functions of a Load Balancer:**
- **Distributes traffic evenly** among multiple servers.
- **Sends traffic only to healthy instances** (performs health checks).
- **Ensures high availability** by rerouting traffic if a server fails.
- **Provides a single point of access** (DNS endpoint) for applications.
- **Supports SSL termination** (decrypts HTTPS traffic before forwarding).
- **Enables session stickiness** (maintains user sessions on the same server using cookies).

---

## **2. Types of Load Balancers in AWS**
AWS offers **four types** of Elastic Load Balancers (ELBs):

1. **Classic Load Balancer (CLB)**
2. **Application Load Balancer (ALB)**
3. **Network Load Balancer (NLB)**
4. **Gateway Load Balancer (GLB)**

Each type operates at different **OSI layers** and has distinct use cases.

---

## **3. Classic Load Balancer (CLB)**
### **Overview**
- The **oldest** type of AWS load balancer.
- Operates at **Layer 4 (Transport Layer - TCP)** and **Layer 7 (Application Layer - HTTP/HTTPS)**.
- Routes traffic between **clients and backend services** (EC2, Containers).

## **3. Classic Load Balancer (CLB)**
### **Overview**
- The **oldest** type of AWS load balancer.
- Operates at **Layer 4 (Transport Layer - TCP)** and **Layer 7 (Application Layer - HTTP/HTTPS)**.
- Routes traffic between **clients and backend services** (EC2, Containers).

### **CLB Components & Limits**
| Component | Limit |
|-----------|-------|
| Listeners | 50 |
| Security Groups | 5 |
| Registered Instances | 1000 |
| Subnets per AZ | 1 |

### **Demo: Creating a Classic Load Balancer**
#### **Step 1: Launch EC2 Instances with Web Servers**
Use **User Data** to install **Nginx** and configure a test page:
```bash
#!/bin/bash
sudo apt-get update
sudo apt-get install nginx -y
sudo echo "this is private IP of machine $(hostname)" > /var/www/html/index.html
```

#### **Step 2: Create a Classic Load Balancer**
1. **Go to EC2 Dashboard → Load Balancers → Create Load Balancer → Classic Load Balancer**.
2. **Select VPC & Availability Zones** (where EC2 instances are running).
3. **Configure Security Group** (allow HTTP/HTTPS traffic).
4. **Configure Listener Ports** (e.g., HTTP:80 → Instance Port:80).
5. **Set Health Check Parameters**:
   - **Response Timeout**: Time to wait for instance response.
   - **Unhealthy Threshold**: Number of failed checks before marking as unhealthy.
   - **Interval**: Time between health checks.
   - **Healthy Threshold**: Number of successful checks before marking as healthy.
6. **Select EC2 Instances** to distribute traffic.
7. **Enable Cross-Zone Load Balancing** (distributes traffic across all AZs).
8. **Enable Connection Draining** (allows in-flight requests to complete before removing an instance).
9. **Review & Create**.

#### **Step 3: Test Load Balancing**
- Access the **CLB DNS name** in a browser.
- Refresh multiple times to see traffic distributed across instances.

---

## **4. Application Load Balancer (ALB)**
### **Overview**
- Operates at **Layer 7 (HTTP/HTTPS/WebSocket)**.
- **Intelligent routing** based on **URL path, hostname, query strings**.
- **Supports microservices** (multiple target groups).
- **Cross-Zone Load Balancing** (enabled by default).
- **Supports SSL termination** (via ACM or third-party certificates).
- **Limitations:**
  - **20 ALBs per region**.
  - **3000 target groups per region**.

### **ALB Components & Limits**
| Component | Limit |
|-----------|-------|
| Listeners | 50 |
| Targets per Load Balancer | 1000 |
| Subnets per AZ | 1 |
| Rules per Listener | 100 |
| Security Groups | 5 |


### **Demo: Creating an ALB**
#### **Step 1: Configure Target Groups**
1. **EC2 → Target Groups → Create Target Group**.
2. **Select Target Type** (Instances, IPs, Lambda).
3. **Set Protocol & Port** (e.g., HTTP:80).
4. **Configure Health Checks** (path: `/`, timeout: 5s).
5. **Register EC2 Instances**.

#### **Step 2: Create ALB**
1. **EC2 → Load Balancers → Create ALB**.
2. **Configure Scheme (Internet-facing/internal)**.
3. **Select VPC & Subnets** (minimum 2 AZs).
4. **Assign Security Group** (allow HTTP/HTTPS).
5. **Configure Listener** (HTTP:80 → Forward to Target Group).
6. **Enable Stickiness (optional)**.
7. **Review & Create**.

#### **Step 3: Path-Based Routing**
- **Create multiple target groups** (e.g., `/app1`, `/app2`).
- **Edit ALB Listener Rules**:
  - If path = `/app1*` → Forward to Target Group 1.
  - If path = `/app2*` → Forward to Target Group 2.

Example **User Data** for path-based routing:
```bash
#!/bin/bash
sudo apt-get update
sudo apt-get install nginx -y
mkdir -p /var/www/html/test
sudo echo "this is private IP of machine $(hostname)" > /var/www/html/test/index.html

```


## **5. Network Load Balancer (NLB)**
### **Overview**
- Operates at **Layer 4 (TCP/UDP)**.
- **Ultra-low latency** (millions of requests/sec).
- **Supports static IP & Elastic IP**.
- **Cross-Zone Load Balancing (disabled by default)**.
- **Use Cases**: Gaming, VoIP, high-throughput apps.


### **NLB Components & Limits**
| Component | Limit |
|-----------|-------|
| Listeners | 50 |
| Targets per AZ (Cross-Zone disabled) | 200 |
| Targets per AZ (Cross-Zone enabled) | 200 |
| Subnets per AZ | 1 |

### **Demo: Testing NLB**
```bash
#!/bin/bash
nwlb="Network Load Balancer DNS"
for ((i=0;i<100;i++))
do
   curl ${nwlb}
done
```

---


## **6. Gateway Load Balancer (GLB)**
### **Overview**
- Operates at **Layer 3 (IP Packets)**.
- **Uses GENEVE protocol (Port 6081)**.
- **Ideal for Firewalls, IDS/IPS, Deep Packet Inspection**.
- **Transparent network gateway**.

### **GLB Components & Limits**
| Component | Limit |
|-----------|-------|
| GLBs per VPC | 10 |
| Target Groups (GENEVE) | 10 |

---

## **7. Comparison: ALB vs NLB vs GLB**
| Feature | ALB | NLB | GLB |
|---------|-----|-----|-----|
| **OSI Layer** | Layer 7 | Layer 4 | Layer 3 |
| **Protocols** | HTTP/HTTPS/WebSocket | TCP/UDP | IP Packets (GENEVE) |
| **Use Case** | Web Apps, Microservices | High-performance apps | Firewalls, IDS/IPS |
| **Cross-Zone LB** | Enabled by default | Disabled by default | N/A |
| **Static IP** | No | Yes | Yes |

---

## **8. Conclusion**
- **Use ALB** for **HTTP/HTTPS routing** (microservices, path-based routing).
- **Use NLB** for **high-performance TCP/UDP** (gaming, VoIP).
- **Use GLB** for **security appliances** (firewalls, IDS).
- **Avoid CLB** (legacy, limited features).


### Shell Script for testing Network Load Balancer

- vi load.sh

```bash
#!/bin/bash

nwlb="Network Load Balancer Hostname"
       for ((i=0;i<100;I++))
do
       curl ${nwlb}
done
```