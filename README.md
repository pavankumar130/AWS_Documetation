# AWS_Documetation

## AWS Identity and Access Management (IAM)
### Overview
AWS Identity and Access Management (IAM) is a service that enables you to securely manage access to AWS resources. IAM allows you to define who (identity) can access which resources and what actions they can perform. It provides a centralized way to manage permissions, authentication, and authorization in your AWS environment.

IAM includes several key components: Users, Groups, Roles, Policies, and Password Policies. Below is a detailed explanation of these components.

### Key Components of AWS IAM

### 1. IAM Users

An IAM user is an identity within your AWS account that represents an individual or application that requires access to AWS resources.

### Attributes:

<li>Each IAM user has a unique name within the AWS account.</li>
<li>Users can have associated security credentials such as passwords (for AWS Management Console access) and access keys (for programmatic access via the AWS CLI, SDKs, or APIs).</li>
<li>Permissions are assigned to users directly or indirectly via groups or roles.</li>

### Example Use Case:

A user, like an employee, might need access to AWS resources such as EC2 or S3 to manage infrastructure.

### 2. IAM Groups

An IAM group is a collection of IAM users who share a common set of permissions. By grouping users into categories, you can assign permissions at the group level rather than individually, simplifying the process of managing permissions for multiple users.

### Attributes:

<li> Groups do not have security credentials. They are simply used to manage permissions for the users that belong to them.</li>
<li>  A user can be a member of multiple groups. </li> 

### Example Use Case:

A "Developers" group could be given permissions to launch EC2 instances, whereas an "Admin" group could have broader permissions such as managing IAM policies.
