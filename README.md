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

### 3. IAM Roles

An IAM role is an AWS identity that you can assume to perform specific tasks. Unlike users, roles are not associated with a specific person or application. Roles are designed to be assumed by trusted entities, including IAM users, AWS services, or federated users.

### Attributes:

<li> Roles have permissions attached to them, similar to users and groups, but they are meant to be assumed temporarily by entities that require access to resources. </li>
<li> Roles can be assumed by IAM users or AWS services to perform specific actions on behalf of the entity. </li>

### Example Use Case:

A role might be assumed by an EC2 instance to allow it to interact with other AWS services such as S3, or it could be used to grant cross-account access, allowing a user in one AWS account to assume a role in another account.

### 4. IAM Policies

An IAM policy is a JSON document that defines permissions. Policies specify what actions are allowed or denied for specific AWS resources.

### Attributes:

<li> Action: Specifies the actions (such as s3:PutObject, ec2:StartInstances) that are allowed or denied. </li>
<li> Resource: Defines the specific AWS resources to which the actions apply (e.g., a specific S3 bucket or EC2 instance). </li>
<li> Condition: (Optional) Defines specific conditions under which the policy is applicable, such as restricting access to certain IP addresses. </li>

### Types of Policies:

<li> Managed Policies: Predefined policies provided by AWS or custom policies created by users. These can be reused across multiple users, groups, or roles. </li>
<li> Inline Policies: Policies that are directly attached to a single user, group, or role, usually for more specific use cases. </li>

### Example Use Case:

A policy that allows a user to upload files to an S3 bucket but not delete them could look like this:

### Example IAM Policy: Allow Upload, Deny Deletion, and Allow Listing for S3 Bucket

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["s3:PutObject"],
            "Resource": "arn:aws:s3:::my-bucket/*"
        },
        {
            "Effect": "Deny",
            "Action": ["s3:DeleteObject"],
            "Resource": "arn:aws:s3:::my-bucket/*"
        },
        {
            "Effect": "Allow",
            "Action": ["s3:ListBucket"],
            "Resource": "arn:aws:s3:::my-bucket"
        }
    ]
}


### 5. IAM Password Policies

IAM password policies define the security requirements for user passwords in your AWS account. Password policies help enforce strong, secure authentication practices.

### Attributes:
<li> You can specify rules for minimum password length, required characters (uppercase, lowercase, digits, special characters), password </li>
<li> expiration, and password reuse prevention.</li>
<li> Multi-factor authentication (MFA) can be required for users accessing the AWS Management Console. </li>

### Example Settings:
<li> Minimum password length: 8 characters </li>
<li> Require at least one uppercase letter, one lowercase letter, one number, and one special character </li>
<li> Password expiration every 90 days </li>
<li> Prevent the reuse of the last 5 passwords </li>