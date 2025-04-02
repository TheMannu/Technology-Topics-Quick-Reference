### 1. **Performing Tasks as a Role in CloudShell**
   - AWS CloudShell automatically inherits the permissions of the IAM user or role that you are logged in as in the AWS Management Console.
   - If you need to perform tasks using a specific IAM role, you can assume that role in the AWS Management Console before launching CloudShell. Once you assume the role, CloudShell will have the permissions associated with that role.
   - This is a secure way to manage AWS resources because you don't need to store long-term credentials (like access keys) on the VM or in your environment.


### 2. **CloudShell is Region-Specific**
   - AWS CloudShell is indeed region-specific. When you launch CloudShell, it is tied to the AWS region that you have selected in the AWS Management Console.
   - If you need to perform tasks in multiple regions, you will need to switch regions in the AWS Management Console and launch a new CloudShell session in each region.


### 3. **Best Practice: Using Roles Instead of Storing Credentials**
   - It is a best practice to use IAM roles rather than storing long-term credentials (like access keys and secret keys) on your virtual machines or in your environment.