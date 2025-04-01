
The AWS IAM Credentials Report is a comprehensive CSV (Excel-compatible) report that provides detailed information about all IAM users and their credentials in your AWS account. This report helps you audit and manage access, identify unused credentials, and enforce security best practices.

## Key Features of AWS Credentials Report

### 1. Lists All IAM Users & Their Credentials

- Access keys (active/inactive)
- Password status (enabled/disabled)
- MFA (Multi-Factor Authentication) status
- Last used timestamps (for passwords & access keys)

### 2. Helps Identify Security Risks

- Unused access keys (potential security risk)
- Users without MFA (violates security best practices)
- Old passwords that haven’t been rotated

### 3. Downloadable in CSV Format

- Can be opened in Excel, Google Sheets, or analyzed programmatically

---

## How to Generate & Download the Credentials Report
### Using AWS Management Console
  1. Go to IAM Dashboard → Credential Report.
  2. Click "Download Report" (if available) or "Generate Report" (if first time).
  3. Once generated, download the CSV file.

  ### Using AWS CLI
  ```bash
  aws iam generate-credential-report   # Generate the report (if not already available)

  aws iam get-credential-report --output text --query Content | base64 --decode > credential-report.csv  # Download the report

  ```

### **What to Analyze in the Report?**
| Column | What It Tells You | Action Required |
|--------|------------------|----------------|
| `user` | IAM username | Check if the user is still active. |
| `password_enabled` | Whether a login password is set | Disable if not needed. |
| `password_last_used` | Last time password was used | Disable if unused for >90 days. |
| `access_key_1_active` / `access_key_2_active` | Whether access keys exist | Disable/Delete unused keys. |
| `access_key_1_last_used_date` | Last time access key was used | Rotate/Delete if unused. |
| `mfa_active` | Whether MFA is enabled | **Enforce MFA** if disabled. |
| `access_key_1_last_rotated` | When the key was last rotated | Rotate keys every 90 days. |

---

### **Common Actions Based on the Report**
✅ **Disable Unused Access Keys** → If `access_key_last_used` is >90 days old.  
✅ **Delete Inactive IAM Users** → If `password_last_used` is too old. 
✅ **Enforce MFA** → For all users with `mfa_active=false`.  
✅ **Rotate Access Keys** → If `access_key_last_rotated` is >90 days.  
✅ **Remove Unnecessary Passwords** → Disable `password_enabled` for service roles.  

---

### **Automating Remediation**
You can automate checks using:
- **AWS Config Rules** (e.g., `iam-user-mfa-enabled`, `access-keys-rotated`)
- **AWS Lambda + EventBridge** (to schedule periodic reviews)  
- **AWS Security Hub** (for centralized compliance tracking)  

---

### **Best Practices**
🔹 **Review the report monthly** (or automate checks).  
🔹 **Follow Least Privilege** – Remove unused permissions. 