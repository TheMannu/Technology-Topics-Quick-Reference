# **Amazon EFS (Elastic File System)**

Amazon EFS (Elastic File System) is a **scalable, shared file storage** service that allows multiple EC2 instances to access the same file system simultaneously. It is **compatible with NFS (Network File System)**, making it ideal for shared workloads like web hosting, content management, and data analytics.

---

## **1. Setting Up EFS with EC2 Instances in Different AZs**
### **Step 1: Create an EFS File System**
1. **Go to AWS Console → EFS → Create file system**  
   - Provide a **name** (e.g., `my-efs`).  
   - Select **VPC** and **subnets** (ensure they match EC2 instances).  
   - **Enable NFS security group** (port **2049** must be open).

2. **Configure Mount Targets**  
   - EFS automatically creates **mount targets** in each **Availability Zone (AZ)**.  
   - Ensure the **EC2 security group allows NFS traffic (TCP 2049)**.

---

### **Step 2: Launch Two EC2 Instances in Different AZs**
- **Instance 1:** `us-east-1a`  
- **Instance 2:** `us-east-1b`  
- Both must have:   
  - **IAM role** with `AmazonEC2FullAccess` (or minimum `AmazonElasticFileSystemClientReadWrite`).  
  - **Security group allowing NFS (port 2049)**.  

---

### **Step 3: Mount EFS on First EC2 Instance (us-east-1a)**
1. **Connect via SSH**  
   ```sh
   ssh -i "key.pem" ec2-user@<Instance1-IP>
   ```
