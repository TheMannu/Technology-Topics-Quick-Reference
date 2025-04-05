# **Enabling Root Login with Password and Adding User Tarun on Amazon Linux**

## **1. Connecting to the Instance**
1. **Connect via SSH** using the key pair:  
   ```bash
   ssh -i key.pem ubuntu@<IP_Address>
   ```
2. **Switch to root user**:  
   ```bash
   sudo -i
   ```

## **2. Enabling Root Login & Password Authentication**
1. **Edit SSH configuration**:  
   ```bash
   vim /etc/ssh/sshd_config
   ```
2. **Make the following changes** (Press `i` to enter insert mode):  
   ```plaintext
   PermitRootLogin yes
   PasswordAuthentication yes

3. **Save and exit** (Press `Esc`, then `:wq!`).  
4. **Set a password for root**:  
   ```bash
   passwd root
   ```
   - Enter the new password twice.  
5. **Restart SSH service**:  
   ```bash
   systemctl restart sshd
   ```

### **Now, root can log in via password:**
```bash
ssh root@<IP_Address>
```
(Enter the password when prompted.)

---


## **3. Adding a New User (Tarun)**
1. **Create a new user**:  
   ```bash
   useradd tarun
   ```
2. **Switch to the new user**:  
   ```bash
   su - tarun
   ```
3. **Generate SSH key pair**:  
   ```bash
   ssh-keygen
   ```
   - Press `Enter` for all prompts (default settings).  
4. **Navigate to `.ssh` directory**:  
   ```bash
   cd ~/.ssh
   ls
   ```
5. **View the public key**:  
   ```bash
   cat id_rsa.pub
   ```
   - **Share this public key with Tarun**.  
6. **Set a password for Tarun**:  
   ```bash
   passwd tarun
   ```
   - Enter the password twice.  

---

## **4. Tarun’s Login Process**
1. **Save the public key** (`id_rsa.pub`) as `key.pem`.  
2. **Connect to the instance**:  
   ```bash
   ssh -i key.pem tarun@<Public_IP>
   ```
3. **Enter the password** when prompted.  

---

## **5. Setting Up a Web Server (Apache HTTPD)**
1. **Install Apache**:  
   ```bash
   yum install httpd -y
   ```
2. **Navigate to web directory**:  
   ```bash
   cd /var/www/html
   ```
3. **Download a website (e.g., WordPress)**:  
   ```bash
   wget <Download_Link>
   ```
4. **Unzip the file**:  
   ```bash
   unzip <filename.zip>
   ```
5. **Move files to root directory**:  
   ```bash
   cp -rvf foldername/* .
   ```
6. **Remove the extracted folder**:  
   ```bash
   rm -rf foldername
   ```
7. **Restart Apache**:  
   ```bash
   systemctl restart httpd
   systemctl enable httpd
   ```




## **Summary of Steps**
| **Task** | **Command** |
|----------|------------|
| **Enable Root Login** | Edit `/etc/ssh/sshd_config` → `PermitRootLogin yes` & `PasswordAuthentication yes` |
