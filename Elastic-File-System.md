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
   mkdir /efs  
   ```
   - Use the **same EFS mount command** (but ensure AZ matches `us-east-1b`).  
   ```sh
   mount -t efs -o tls fs-12345678:/ /efs  
   ls /efs   # Should show files created earlier (file1, file2, etc.)
   ```

✅ **Now both instances share the same storage!** Changes in one appear in the other.

---

## **2. Setting Up an NFS Server (Alternative to EFS)**
If you prefer a **self-managed NFS server**, follow these steps:

### **Step 1: Install NFS on Server 1 (NFS Host)**
1. **Check if NFS is installed**  
   ```sh
   yum list installed | grep nfs-utils  
   yum list installed | grep rpcbind  
   ```
   - If not installed:  
     ```sh
     yum install nfs-utils rpcbind -y  
     ```

2. **Create Shared Directory**  
   ```sh
   mkdir /share  
   touch /share/file.txt  
   ```

3. **Configure NFS Exports**  
   ```sh
   vim /etc/exports  
   ```
   - Add:  
     ```
     /share <IP-of-Server2>(rw,sync)  
     ```
   - Save (`:wq!`).

4. **Start & Enable NFS Services**  
   ```sh
   systemctl start nfs rpcbind  
   systemctl enable nfs rpcbind  
   systemctl status nfs  
   ```

5. **Set Permissions**  
   ```sh
   chmod -R 777 /share  
   exportfs -rav  # Reload exports  
   ```

