The **`stress`** command is a Linux tool designed to put the system under stress by generating high CPU, memory, I/O, or disk usage. It's often used for performance testing, stress testing, or diagnosing hardware issues under load.

---

### **Installing `stress`**
For most Linux distributions, you can install it using the package manager:

- **Ubuntu/Debian**:  
  ```bash
  sudo apt update && sudo apt install stress
  ```

- **RHEL/CentOS**:  
  ```bash
  sudo yum install epel-release
  sudo yum install stress
  ```

---

### **Basic Usage**
Run `stress` with options to specify the type and level of stress you want to induce.

#### Common Options:
| Option              | Description                                     |
|---------------------|-------------------------------------------------|
| `-c <N>`            | Stress with `<N>` CPU workers                   |
| `-m <N>`            | Stress with `<N>` memory workers                |
| `--vm-bytes <SIZE>` | Allocate `<SIZE>` of memory per VM worker       |
| `-i <N>`            | Stress with `<N>` I/O workers                   |
| `-d <N>`            | Stress with `<N>` disk workers                  |
| `-t <TIME>`         | Run stress test for `<TIME>` seconds            |

---

### **Examples**
1. **CPU Stress Test**:  
   Run 4 CPU workers indefinitely:  
   ```bash
   stress -c 4
   ```

2. **Memory Stress Test**:  
   Allocate 1GB of memory using 2 workers:  
   ```bash
   stress -m 2 --vm-bytes 1G
   ```

3. **I/O Stress Test**:  
   Use 2 I/O workers to stress the system:  
   ```bash
   stress -i 2
   ```

4. **Mixed Load Test**:  
   Combine CPU, memory, and I/O stress tests for 60 seconds:  
   ```bash
   stress -c 2 -m 1 --vm-bytes 512M -i 1 -t 60
   ```

5. **Disk Stress Test**:  
   Use 2 disk workers:  
   ```bash
   stress -d 2
   ```

---

### **Monitoring System During Stress**
To observe system performance while running `stress`, use tools like:
- **`top`**: Real-time process monitoring.
- **`htop`**: Enhanced, interactive process viewer.
- **`iotop`**: Monitor I/O usage by processes.
- **`free -h`**: Check memory usage.
- **`sar`** (from sysstat): Detailed performance metrics.

### **Stopping Stress**
Press **Ctrl+C** to stop the `stress` test manually. 