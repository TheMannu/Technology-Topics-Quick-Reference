## 1. AWS Access Analyzer
AWS Access Analyzer helps identify unintended access to your resources by analyzing resource-based policies (e.g., S3 buckets, KMS keys, IAM roles, Lambda functions, etc.). It detects external access that may violate security best practices.

### Key Features:
  - Identifies External Access: Finds resources shared with external entities (e.g., another AWS account, a public user, or an anonymous user).

  - Policy Validation: Checks policies for errors and security risks.

  - Findings Dashboard: Provides a list of findings with remediation steps.

  - Continuous Monitoring: Continuously monitors for new or updated policies that may expose resources.

### Use Cases:
  - Detect if an S3 bucket is accidentally made public.

  - Identify an IAM role that grants unnecessary permissions to an external AWS account.

  - Validate new policies before deployment.


## 2. IAM Access Advisor
Access Advisor provides last accessed information for IAM users, groups, and roles, showing which AWS services and actions were actually used.

### Key Features:
  - Service-Level Access Tracking: Shows when an IAM entity (user/role/group) last accessed an AWS service.
  - Permission Optimization: Helps remove unused permissions (least privilege principle).
  - Time-Based Insights: Tracks access over the last 365 days.


### Use Cases:
  - Identify if a role has permissions to EC2 but hasn’t used them in months → Remove unnecessary permissions.

  - Check if a user has S3 permissions but has never accessed S3 → Justify or revoke access.

  - Audit permissions before enforcing stricter policies.

## How They Work Together for Security & Compliance
1. Access Analyzer → Finds unintended external access to resources.

2. Access Advisor → Checks if internal users/roles are actually using their permissions.

3. Justification & Remediation:

  - If Access Advisor shows unused permissions, you can remove them.

  - If Access Analyzer detects unintended external access, you can restrict it.

  - You can ask teams for justification if permissions seem unnecessary.

### Best Practices
✅ Regularly Review Access Advisor – Remove unused permissions to follow least privilege.
✅ Enable Access Analyzer – Continuously monitor for unintended external access.
✅ Use AWS IAM Policy Simulator – Test policies before applying changes.
✅ Automate with AWS Organizations & SCPs – Enforce guardrails across accounts.
