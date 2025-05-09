## IAM ( Identity and Access Management ) 
- AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. With IAM, you can manage permissions that control which AWS resources users can access. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources. IAM provides the infrastructure necessary to control authentication and authorization for your AWS accounts.

- As a best practice we should follow Least Privileged Permission Polices - and can be checked using Access Report - Access Analyzer or Credential Report. 


### Difference between root User and User
1. account level configuration(contact info, etc.) can not  be done by IAM admin and it can only be done by root user.

2. Suppose by chance if admin user(A) has removed its own permission by mistake and in this case only root user is there who can give back the admin permission to A.

3. Billing and cost management access is not assigned to admin user by default but these access are assign to root user by default.
4. Only root user can close your AWS account and IAM admin can't close the account.

5. Support plan can only be changed by root user and not by IAM admin.

## Conflicts Of  Allow and Deny
In AWS Identity and Access Management (IAM), when a user is associated with multiple groups, the permissions are cumulative. This means that if you attach a user to two groups—one with permissions to read and write in Amazon S3, and another with explicit denials to read and write in S3—the user will ultimately be denied those permissions. 

In IAM, explicit denials take precedence over permissions granted.  This is known as the "deny overrides" principle.So, if any group the user belongs to denies a certain permission, even if the other groups grant that permission, the denial will take precedence and the user will be denied access.

In your scenario:
- Group 1 grants read and write permissions to S3.
- Group 2 denies read and write permissions to S3.

If you attach both groups to a user, the user will not have permissions to read and write in S3. The explicit denial from Group 2 will override the permissions from Group 1.

So, the user will essentially have the least permissive set of permissions across all the groups they are associated with. In this case, the user would not be able to perform any S3 actions due to the explicit denial in Group 2.

## Password Policies

Brut Force Attach - A brute force attack is a hacking method that uses trial and error to crack passwords, login credentials, and encryption keys.

To keep Our account safe from these types of attacks 
We can Configure or Customize IAM Account Pasword Policies from 
- IAM dashboard - Access Management - Account Setting - Edit Password Policy - Custom - Configure As per Our Requirements - Save Changes

We can Also Enable MFA (Multi-Factor Authentication)
- IAM dashboard - Add MFA
The MFA keeps us safe from attacks that is done by Key Logger using background running program to send key logs to hackers
That's why bank account login pages do have Virtual Keyboard to type using Mouse and  avoid Key Logger Attack

## AWS CLI Configuration - aws configure 

We can Create resources on AWS from CLI also and for that we need to configure AWS CLI in our Local System 
- IAM dashboard - User - Select User - Security Credentials - Access Keys (Max Two) - Create Access Keys - Download CSV file - Done

Now Install CLI in our Local Machine Linux, Windows or Mac
- Now open The CLI and Run Command - "aws --version" - "aws configure" - paste the Access Keys - Paste the Secret Access Keys - May Select Regions - May Select Output format.
- Check With command - "AWS help" 

To Create An instance - 
Run - "aws ec2 run-instance --image-id=<Value> --instance-type=t2.micro --region=ap-south-1"

example - "aws ec2 run-instances --image-id=ami-09a9858973b288bdd --instance-type=t3.micro --region=eu-north-1"

To Check The available Instances - 
Run - "aws ec2 describe-instances"


## Configure Multiple 
command - "aws configure --profile acc1" 
Run - "aws ec2 describe-instances --profile acc1"

Configure Default Account
command - "aws configure --profile profileName" - "export AWS_DEFAULT_PROFILE=profileName"

## IAM - Role
A role is an IAM identity that you can create in your account that has specific permissions. An IAM role has some similarities to an IAM user. Roles and users are both AWS identities with permissions policies that determine what the identity can and cannot do in AWS.However, instead of being uniquely associated with one person, a role can be assumed by anyone who needs it. A role does not have standard long-term credentials such as a password or access keys associated with it. Instead, when you assume a role, it provides you with temporary security credentials for your role session.

We can use roles to delegate access to users, applications, or services that don't normally have access to your AWS resources.

The types of AWS Identity and Access Management (IAM) roles are categorized by their trust policies. The main types of IAM roles are:

• Service role: An application running on an EC2 instance can assume this role to perform actions in your account.
• Service-linked role: This role is owned by AWS and includes all the permissions required to call other AWS services on your behalf.
• Web Identity role: A type of IAM role.
• SAML 2.0 federation role: A type of IAM role.
• Custom IAM role: A type of IAM role.

IAM roles are associated with permissions that determine access within AWS. They are designed to be assumed by a user, service, or application to get temporary access to AWS resources.

1. Service Role
• Definition: A role assumed by an application or service running on an EC2 instance.
• Purpose: It allows the EC2 instance to securely access and perform actions on AWS services (like reading from an S3 bucket, writing logs to CloudWatch, etc.) without requiring hard-coded credentials in the application.

• Example Use Case:
An application running on an EC2 instance assumes the service role to upload files to an S3 bucket or retrieve secrets from AWS Secrets Manager.

2. Service-Linked Role
• Definition: A role owned and managed by AWS that includes all permissions necessary for a specific AWS service to call other AWS services on your behalf.

• Key Features:
  - AWS creates, manages, and attaches policies to the role automatically.
  - Cannot be manually deleted or modified unless the associated service is removed.

• Example Use Case:
  - Amazon EC2 Auto Scaling creates a service-linked role that allows it to modify instance configurations, attach instances to load balancers, etc.

3. Web Identity Role
• Definition: A type of IAM role that allows federation with web identity providers (like Google, Facebook, or Amazon Cognito).
• Purpose: This role is often used in mobile or web applications to authenticate users via an identity provider and grant them temporary credentials for AWS services.
• Example Use Case:
  - A mobile app using Amazon Cognito for authentication allows users to upload photos to an S3 bucket, with the app assuming the Web Identity role.

4. SAML 2.0 Federation Role
• Definition: A type of IAM role used for federated authentication with SAML 2.0-compatible identity providers (e.g., Microsoft Active Directory, Okta).
• Purpose: Enables users to log in to AWS via an enterprise identity provider and assume roles with appropriate permissions.
• Example Use Case:
  - An employee logs into the company’s internal portal (using AD or Okta), and a SAML assertion grants them temporary access to AWS Management Console with the privileges associated with a specific role.
  
5. Custom IAM Role
• Definition: A role created and defined manually by you (the user), tailored to meet specific permission requirements.
• Purpose: Allows precise control over what actions can be performed on which AWS resources. 
• Example Use Case:
  - A custom role is created for a Lambda function that grants permissions only to read/write data to specific DynamoDB tables or invoke specific AWS APIs. 

### Comparison at a Glance:

| **Role Type**          | **Who Owns It?**   | **Usage**                                      | **Example**                                                |
|------------------------|-------------------|------------------------------------------------|------------------------------------------------------------|
| **Service Role**       | User-defined      | Application on EC2 to access AWS services.     | An EC2 instance writes logs to CloudWatch.                |
| **Service-Linked Role** | AWS               | AWS services perform actions on your behalf.   | Auto Scaling service modifying instance counts.            |
| **Web Identity Role**  | User-defined      | Federation with web identity providers.        | Users authenticated via Cognito upload photos to S3.      |
| **SAML 2.0 Federation** |	User-defined	 | Federation with enterprise identity providers (e.g., Okta).| Employee logs into AWS via an enterprise portal.
| **Custom IAM Role** |	User-defined	 | Tailored permissions for specific use cases.	| A Lambda function reads/writes from a DynamoDB table.

---
Each role type serves a specific purpose, ensuring secure and fine-grained access to AWS resources in a variety of scenarios.

---


### Question 1 : How can you create a user in AWS with full EC2 access limited to a specific region?

Steps to Create a User with Region-Specific Full EC2 Access

1. Create an IAM User:

   - Go to the IAM section in the AWS Management Console.
   - Create a new user and select Programmatic access or Management Console access, depending on your needs.
   - Attach permissions later (we'll cover this in the next step).
   
2. Define a Custom Policy: You need two policy statements:

   - One to allow full EC2 access in the desired region.
   - Another to explicitly deny access to EC2 resources in all other regions.


  ```yaml
  {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:*",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": "us-west-2"
                }
            }
        },
                {
            "Effect": "Deny",
            "Action": "ec2:*",
            "Resource": "*",
            "Condition": {
                "StringNotEquals": {
                    "aws:RequestedRegion": "us-west-2"
                }
            }
        }
    ]
  }
  ```

- Explanation:
  - The first statement explicitly allows EC2 actions only in the us-west-2 region.
  - The second statement denies EC2 actions in any region except us-west-2.

3. Attach the Policy to the User:

   - Go to the IAM user you created.
   - Attach the custom policy (either directly or via a group).

4. Test the Configuration:

   - Log in as the IAM user (or use their access credentials) and attempt to perform EC2 operations in us-west-2 (should succeed).
   - Try accessing EC2 resources in other regions (should be denied).

## Key Notes:
- Condition Key Used:
  - aws:RequestedRegion is used to specify the region in API requests. AWS ensures the user can only perform actions in the allowed region.
- Restricting Resources:
  - The Resource: "*" field can be further restricted to specific EC2 resources (like instances, volumes, etc.) if needed.
- Policy Priority:
  - Explicit denies (as in this policy) always override allows, ensuring no accidental access to other regions.

### 1. **Performing Tasks as a Role in CloudShell**
   - AWS CloudShell automatically inherits the permissions of the IAM user or role that you are logged in as in the AWS Management Console.
   - If you need to perform tasks using a specific IAM role, you can assume that role in the AWS Management Console before launching CloudShell. Once you assume the role, CloudShell will have the permissions associated with that role.
   - This is a secure way to manage AWS resources because you don't need to store long-term credentials (like access keys) on the VM or in your environment.

### 2. **CloudShell is Region-Specific**
   - AWS CloudShell is indeed region-specific. When you launch CloudShell, it is tied to the AWS region that you have selected in the AWS Management Console.
   - If you need to perform tasks in multiple regions, you will need to switch regions in the AWS Management Console and launch a new CloudShell session in each region.

### 3. **Best Practice: Using Roles Instead of Storing Credentials**
   - It is a best practice to use IAM roles rather than storing long-term credentials (like access keys and secret keys) on your virtual machines or in your environment.

   - IAM roles provide temporary security credentials that are automatically rotated, reducing the risk of credential exposure.

   - By using roles, you can follow the principle of least privilege, granting only the permissions necessary to perform specific tasks.

## Example: Assuming a Role in CloudShell
### 1. **Assume the Role in the AWS Management Console:**

   - Navigate to the IAM service in the AWS Management Console.
   - Switch to the role you want to assume by clicking on your username in the top-right corner and selecting "Switch Role."
   - Enter the account ID, role name, and optionally a display name for the role.

### 2. **Launch CloudShell:**

   - After assuming the role, launch CloudShell from the AWS Management Console.

   - CloudShell will now have the permissions associated with the role you assumed.

### 2. **Perform Tasks:**

   - You can now use AWS CLI commands or other tools within CloudShell to perform tasks with the permissions of the assumed role.

## Benefits of Using CloudShell:
  - No Setup Required: CloudShell comes pre-installed with popular tools like the AWS CLI, SDKs, and other utilities.

  - Secure: CloudShell automatically uses the credentials of the IAM user or role you are logged in as, so you don't need to manage or store additional credentials.
  - Integrated: Since it's integrated into the AWS Management Console, you can easily switch between regions and services.

## Limitations:
- Session Duration: CloudShell sessions have a maximum duration (currently 20 minutes of inactivity or 12 hours total, whichever comes first).

- Region-Specific: As mentioned, CloudShell is tied to the region you are working in, so you need to launch a new session if you switch regions.

AWS CloudShell with IAM roles is a secure and efficient way to manage your AWS resources without the need to store long-term credentials on your virtual machines.


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