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

2. **Install EFS Utilities**  
   ```sh
   sudo -i  
   apt update -y  
   apt install amazon-efs-utils -y  # Installs EFS mount helper

3. **Create Mount Directory**  
   ```sh
   mkdir /efs  
   ```

4. **Mount EFS**  
   - Go to **AWS EFS Console → Select EFS → Attach**.  
   - Copy the **NFS mount command** (e.g., for `us-east-1a`). 
   - Example command:  
     ```sh
     mount -t efs -o tls fs-12345678:/ /efs
     ```
   - Verify mounting:  
     ```sh
     mount -s | grep efs  # Check if mounted
     df -h                # Verify storage
     ```

5. **Set Permissions & Test**  
   ```sh
   sudo chmod 777 /efs   # Allow read/write for all users  
   cd /efs  
   touch file{1..6}      # Create test files  
   ls                    # Verify files exist
   ```

---

### **Step 4: Mount EFS on Second EC2 Instance (us-east-1b)**
1. **Connect via SSH**  
   ```sh
   ssh -i "key.pem" ec2-user@<Instance2-IP>
   ```

2. **Install EFS Utilities & Mount**  
   ```sh
   sudo -i  
   apt update -y  
   apt install amazon-efs-utils -y  

