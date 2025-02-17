Here’s a streamlined guide for installing Jenkins on an Ubuntu system:

### Step 1: Install Java

1. **Update Package List:**
   ```bash
   sudo apt update
   ```

2. **Install Java Development Kit (JDK):**
   ```bash
   sudo apt-get install openjdk-11-jdk-headless
   ```

3. **Verify Java Installation:**
   ```bash
   java -version
   ```

### Step 2: Add Jenkins Repository Key

1. **Download and Add Jenkins Repository Key:**
   ```bash
   curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
   ```

### Step 3: Add Jenkins Repository

1. **Add Jenkins Repository to Sources List:**
   ```bash
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
   ```

### Step 4: Install Jenkins

1. **Update Package List:**
   ```bash
   sudo apt update
   ```

2. **Install Jenkins:**
   ```bash
   sudo apt install jenkins
   ```

### Step 5: Verify Jenkins Installation

1. **Check Jenkins Service Status:**
   ```bash
   sudo systemctl status jenkins
   ```

### Step 6: Access Jenkins

1. **Open Jenkins in a Browser:**
   - Visit: `http://localhost:8080` or `http://<your-ip-address>:8080`

### Additional Steps

1. **Configure Security Group/Firewall:**
   - Ensure the inbound rule allows traffic on TCP port 8080.

2. **Retrieve Initial Admin Password:**
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

This should give you a complete setup for Jenkins on Ubuntu!