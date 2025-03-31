
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