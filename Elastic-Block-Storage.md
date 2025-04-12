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

## **3. EBS vs. Instance Store (Ephemeral Storage)**
| Feature | **EBS (Persistent Storage)** | **Instance Store (Temporary Storage)** |
|---------|-----------------------------|----------------------------------|
| **Persistence** | Data remains after instance stop/termination | **Data is lost** on stop/restart |
| **Performance** | Slightly slower (network-based) | **Faster** (directly attached) |
| **Use Case** | Long-term storage (OS, databases) | **Temporary data** (caching, buffers) |

---

## **4. Attaching & Configuring EBS Volumes**
### **a) Attaching an EBS Volume**
1. **Create an EBS volume** in the **same AZ** as the EC2 instance.
2. **Attach** via AWS Console:  
   - Select EBS → **Actions → Attach Volume**.
3. **On EC2 (Linux CLI)**:
   ```sh
   lsblk                         # Check available block devices
   sudo mkfs.ext4 /dev/xvdf      # Format EBS (ext4 filesystem)
   sudo mkdir /test              # Create mount directory
   sudo mount /dev/xvdf /test    # Mount EBS
   mountpoint /test              # Verify mounting
   ```

### **b) Detaching an EBS Volume**
1. **Unmount** in CLI:
   ```sh
   umount /test/
   ```

2. **Detach via AWS Console**.
3. **Reattach to another EC2** (must be in the same AZ).

### **c) Resizing & Expanding File System**
- **Increase EBS size** via AWS Console (**Modify Volume**).
- **Check new size**:
  ```sh
  lsblk      # Shows new size
  df -h      # Still shows old size (needs resizing)
  ```

- **Resize filesystem**:
  ```sh
  resize2fs /dev/xvdf   # Expands filesystem
  ```

---

## **5. Multi-Attach EBS (io1 & io2)**
- **Only io1 & io2** support **multi-attach**.
- **Requirements**:
  - EC2 must use **Nitro Hypervisor** (not Xen).
  - **File system must be cluster-aware** (regular ext4/xfs may not work).
- **Steps**:
  1. **Stop EC2** if using Xen.
  2. **Change instance type** to **Nitro-based**.
  3. **Attach EBS to multiple EC2** instances.

---

## **6. EBS Volume Types & Performance**
| **Volume Type** | **gp3** | **gp2** | **io2 Block Express** | **io1** |
|----------------|---------|---------|----------------------|--------|
| **Max Size** | 16 TiB | 64 TiB | 16 TiB | 64 TiB |
| **Max IOPS** | 16,000 | 16,000 | 256,000 | 64,000 |
| **Max Throughput** | 1,000 MiB/s | 250 MiB/s | 4,000 MiB/s | 1,000 MiB/s |
| **Multi-Attach** | ❌ No | ❌ No | ✅ Yes | ✅ Yes |


📌 **Reference**: [AWS EBS Volume Types](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html)

---

## **7. Snapshots & Backup**
### **a) What is a Snapshot?**
- **Point-in-time backup** of an EBS volume.
- Stored in **Amazon S3** (incremental backups).
- Used to **restore EBS** in **different AZ/Region**.

### **b) Managing Snapshots**
- **Lifecycle Manager**: Automates **scheduled snapshots**.
- **Recycle Bin**: Temporarily stores **deleted snapshots** (retention period).
- **Copying Snapshots**:
  - **Action → Copy Snapshot → Select Region**.
- **Encryption**:
  - If **source EBS is encrypted**, snapshot & restored volumes **remain encrypted**.
  - **Cannot decrypt** an encrypted snapshot (must create a new unencrypted one).

### **c) Fast Snapshot Restore (FSR)**
- **Speeds up EBS restoration** from snapshots.
- Must be **enabled per snapshot**.
- Works for volumes **up to 16 TiB**.

---

## **8. Encryption in EBS**
- **Data at rest**: Encrypted using **AWS KMS**.
- **Data in transit**: Encrypted if EBS is encrypted.
- **Default Encryption** can be enabled in **EC2 settings**.
- **Once encrypted, cannot be reversed** (must create a new unencrypted volume).

---

# **Conclusion**
- **EBS** provides **persistent, scalable, and high-performance** storage.
- Supports **multi-attach (io1/io2)**, **snapshots**, and **encryption**.
- Must reside in the **same AZ** as EC2 (use **snapshots** to migrate).
- **Instance Store** is faster but **ephemeral** (data lost on stop).

