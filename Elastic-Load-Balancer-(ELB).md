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
