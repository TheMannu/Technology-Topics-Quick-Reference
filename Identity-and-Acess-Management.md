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