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

### **Step 2: Configure Firewall (If NFS Fails)**
If `showmount -e <Server1-IP>` fails, allow NFS traffic:  
```sh
firewall-cmd --permanent --add-service=nfs  
firewall-cmd --permanent --add-service=2049/tcp  
firewall-cmd --permanent --add-service=rpc-bind  
firewall-cmd --permanent --add-service=mountd  
firewall-cmd --reload  
firewall-cmd --list-all  
```

### **Step 3: Mount NFS on Server 2 (Client)**
1. **Check Available Shares**  
   ```sh
   showmount -e <Server1-IP>  
   ```
   - Output:  
     ```
     Export list for 192.168.1.100:  
     /share 192.168.1.101/24  
     ```

2. **Mount the NFS Share**  
   ```sh
   mkdir /access  
   mount -t nfs <Server1-IP>:/share /access  
   df -hT  # Verify mount  
   ```
   - Now `/access` contains files from `/share`.

---

## **3. Key Differences: EFS vs. NFS**
| Feature | **Amazon EFS** | **Self-Managed NFS** |
|---------|--------------|------------------|
| **Scalability** | Automatic (grows with usage) | Manual (depends on server storage) |
| **Availability** | Multi-AZ (highly available) | Single server (needs backup) |
| **Performance** | Optimized for cloud workloads | Depends on server hardware |
| **Management** | Fully managed by AWS | Requires manual setup & maintenance |
| **Cost** | Pay per GB used | Lower cost but requires admin effort |

---

## **4. Practical Use Cases**
- **EFS**:  
  - Shared storage for **web servers** (WordPress, Drupal).  
  - **Big Data** (Hadoop, Spark shared datasets).  
  - **CI/CD pipelines** (shared build artifacts).  

- **NFS Server**:  
  - **On-premises** file sharing.  
  - **Legacy applications** requiring NFS.  

---

## **Conclusion**
- **EFS** is **easier to set up**, **scalable**, and **fully managed**.  
- **Self-managed NFS** is **cheaper** but requires **manual configuration**.  
- Both allow **shared storage** across multiple instances.  

🔹 **For AWS cloud workloads, EFS is recommended** due to its simplicity and reliability.  
🔹 **For on-premises or custom setups, NFS may be preferred**.

---

## **1. Deep Dive into EFS Architecture**
### **How EFS Works Under the Hood**
- EFS uses **NFSv4.1 protocol** (with support for NFSv4.0)
- Built on **AWS's highly durable storage backend** (same as S3)
- **Multi-AZ by default** - data is replicated across 3+ AZs
- **Two performance modes**:
  - **General Purpose** (default, low latency)
  - **Max I/O** (higher throughput but slightly higher latency)

### **Throughput Modes**
| Mode | Description | Use Case |
|------|------------|----------|
| **Bursting** | Baseline throughput scales with storage size (1MB/s per GB up to 100MB/s) | Small to medium workloads |
| **Provisioned** | Fixed throughput independent of storage size | Predictable high-performance needs |

*Example:*  
A 100GB EFS volume in bursting mode gets **100MB/s baseline** burstable to **300MB/s** temporarily.

## **2. Advanced Mount Options**
### **Mount Command Breakdown**
```bash
sudo mount -t efs -o tls,iam fs-12345678:/ /mnt/efs
```
- `tls`: Enables encryption in transit
- `iam`: Uses IAM for authentication
- `noresvport`: Helps with connection failover

### **Fstab Configuration for Persistent Mounts**
```bash
fs-12345678.efs.us-east-1.amazonaws.com:/ /mnt/efs efs _netdev,tls,iam 0 0
```
- `_netdev`: Ensures mount waits for network availability
- Best practice: Use **DNS name** instead of IP for reliability

## **3. Performance Optimization**
### **Benchmarking Your EFS**
```bash
# Test write performance
dd if=/dev/zero of=/mnt/efs/testfile bs=1G count=1 oflag=direct

# Test read performance
hdparm -Tt /mnt/efs/testfile
```

### **Performance Tips**
1. **File Sizing**:  
   - EFS performs best with **larger files** (>1MB)
   - Avoid millions of tiny files

2. **Concurrency**:  
   - Scale out with multiple EC2 instances for better throughput
   - Each instance gets its own network throughput

3. **Caching**:  
   - Use **Amazon EFS Access Points** with different caching policies
   - Consider **CloudFront** for static content

## **4. Security Best Practices**
### **Encryption Options**
| Type | Implementation | Notes |
|------|---------------|-------|
| **At Rest** | KMS (AWS-managed or CMK) | Enabled at filesystem creation |
| **In Transit** | TLS 1.2+ | Enabled via mount options |

### **Permission Management**
```bash
# Create access point with restricted permissions
aws efs create-access-point \
    --file-system-id fs-12345678 \
    --posix-user Uid=1000,Gid=1000 \
    --root-directory "Path=/project-a,CreationInfo={OwnerUid=1000,OwnerGid=1000,Permissions=750}"
```
