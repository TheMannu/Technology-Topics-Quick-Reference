# **Amazon EBS (Elastic Block Storage)**

## **1. Introduction to EBS**
- **EBS (Elastic Block Storage)** is a **block-level storage service** provided by AWS.
- It operates within a **SAN (Storage Area Network)**, meaning storage is **network-attached** rather than directly connected to the EC2 instance.
- EBS volumes can be **mounted** on EC2 instances and used as **persistent storage**.

---

## **2. Key Characteristics of EBS**
### **(a) EBS as Root Volume**
- In **Windows**, the **C:** drive is typically an EBS volume.
- In **Linux**, it is called the **Root Volume**.
- The **operating system (OS)** is installed on this **root volume (EBS)**.
- The EBS volume is **attached to EC2 via SAN (Storage Area Network)**.

### **b) Performance & Latency**
- **Read/Write operations** on EBS have **minimal latency**, though it is **slightly slower** than instance store (ephemeral storage).
- The **latency is negligible** for most workloads.
