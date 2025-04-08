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

### **c) Persistence & Multi-Attach**
- **Data persists** even if the EC2 instance is **stopped or terminated**.
- **Multiple EBS volumes** can be attached to a **single EC2 instance**.
- **Specific EBS types (io1 & io2)** support **multi-attach**, meaning they can be **attached to multiple EC2 instances simultaneously**.

### **d) Availability Zone (AZ) Dependency**
- EBS volumes **must be in the same AZ** as the EC2 instance to be attached.
- To **move EBS to another AZ**, a **snapshot backup** must be taken and restored in the new AZ.

### **e) Resizing EBS Volumes**
- **EBS volumes can be increased** in size **without stopping** the instance.
- **Decreasing size is not allowed** (AWS does not support downgrading).
- **Workaround for reducing size**:
  - Create a **new smaller EBS volume**.
  - **Copy data** from the larger volume to the smaller one.
  - **Unmount the old volume** and **mount the new one**.

---
