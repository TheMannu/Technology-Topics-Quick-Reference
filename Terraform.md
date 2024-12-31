### OS - Amazon Linux

```
sudo yum install -y yum-utils
```
```
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
```
```
sudo yum -y install terraform
```
```
terraform -help
```

## OS Windows:

### 1. **Download Terraform Binary**
   - Visit the [HashiCorp Terraform Download Page](https://developer.hashicorp.com/terraform/downloads) for Windows.
   - Download the appropriate `Windows 64-bit` binary.

### 2. **Extract the Downloaded File**
   - After downloading, extract the zip file to a folder (e.g., `C:\Terraform`).

### 3. **Copy the Path**
   - Navigate to the folder where Terraform is extracted.
   - Click on the `terraform.exe` file and copy the full path (e.g., `C:\Terraform`).

### 4. **Set Environment Variable**
   - In the **Windows Search Bar**, type "Environment Variables" and click on **Edit the system environment variables**.
   - In the **System Properties** window, click the **Environment Variables** button.
   - Under **User variables for [Your Username]**, select the `Path` variable, then click **Edit**.
   - Click **New** and paste the copied path (e.g., `C:\Terraform`).
   - Click **OK** on all the windows to save the changes.

### 5. **Verify the Installation**
   - Open **Command Prompt** (CMD).
   - Run the following command to verify Terraform is installed:
     ```bash
     terraform --version
     ```
   - This should display the installed version of Terraform, confirming that it was installed successfully.

---

Download Binary  For windows form developers.hasicorp.com
extract
open click on terraform 
copy path 
 
go to environment variable using SEARCH bar
click on ENVIRONMENT variable 
in USER variable for ppara choose PATH
Create NEW path 
Paste the copied path 
NOw Select OK -> Ok -> OK 

verify the Installation 
Open CMD 
Run Command  -> terraform version