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