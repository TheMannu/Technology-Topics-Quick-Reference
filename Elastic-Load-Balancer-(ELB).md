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
