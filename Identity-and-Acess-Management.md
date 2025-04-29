## IAM ( Identity and Access Management ) 
- AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. With IAM, you can manage permissions that control which AWS resources users can access. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources. IAM provides the infrastructure necessary to control authentication and authorization for your AWS accounts.

- As a best practice we should follow Least Privileged Permission Polices - and can be checked using Access Report - Access Analyzer or Credential Report. 


### Difference between root User and User
1. account level configuration(contact info, etc.) can not  be done by IAM admin and it can only be done by root user.

2. Suppose by chance if admin user(A) has removed its own permission by mistake and in this case only root user is there who can give back the admin permission to A.