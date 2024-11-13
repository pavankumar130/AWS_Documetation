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
```

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

## AWS EC2 Overview

Amazon Elastic Compute Cloud (EC2) is a scalable computing service provided by AWS that allows users to run virtual machines (called instances) on-demand. EC2 offers various types of instances optimized for different workloads, such as general computing, memory-intensive, and compute-intensive tasks.

EC2 instances are backed by other AWS services that enhance their functionality and security. Some of these services include Security Groups, Placement Groups, Elastic Network Interfaces (ENIs), Elastic Block Store (EBS), Amazon Machine Images (AMIs), and Elastic File System (EFS). Each of these components serves a specific purpose in ensuring high availability, security, and scalability for EC2 instances.

### Key EC2 Components

### 1. Security Groups

Security Groups in AWS EC2 act as virtual firewalls to control inbound and outbound traffic to EC2 instances. They are stateful, meaning if you allow inbound traffic to a specific port, the response is automatically allowed regardless of outbound rules.

### Attributes:

<li>Security groups control network access at the instance level.
<li>Rules are defined by IP protocol (e.g., TCP, UDP), port range (e.g., 22 for SSH, 80 for HTTP), and source/destination IP address or CIDR block.
<li>You can attach multiple security groups to an EC2 instance.
<li>Security groups are stateful: If you allow inbound traffic, the corresponding outbound traffic is automatically allowed.
<li>Security groups are applied to EC2 instances dynamically; changes are automatically applied.

### Example Use Case:

You can create a security group to allow inbound SSH traffic on port 22 from a specific IP address and HTTP traffic (port 80) from anywhere.

### Example Rule:

<li>Allow inbound traffic on port 22 (SSH) from the IP 192.168.1.1/32.
<li>Allow inbound traffic on port 80 (HTTP) from anywhere (0.0.0.0/0).

### 2. Placement Groups

A Placement Group is a logical grouping of instances within a single Availability Zone (AZ). Placement groups are used to influence the placement of EC2 instances on physical hardware to meet specific performance needs.

There are three types of placement groups:

<li>Cluster Placement Group: Packs instances close together within a single AZ to provide low-latency network performance. Suitable for high-performance computing (HPC) applications.

<li>Spread Placement Group: Distributes instances across underlying hardware to reduce the risk of simultaneous failures. Ideal for critical applications where high availability is important.

<li>Partition Placement Group: Divides instances into logical partitions, with each partition being placed on separate racks. Useful for large distributed systems that require isolation between partitions (e.g., Hadoop).

### Example Use Case:

A Cluster Placement Group is used when you need instances to be placed on the same physical hardware for low-latency communication, such as in high-performance computing scenarios.

### 3. Elastic Network Interface (ENI)

An Elastic Network Interface (ENI) is a virtual network interface that can be attached to an EC2 instance. It provides network connectivity to an instance, enabling communication with other instances or external networks.

### Attributes:

<li>ENIs are associated with an instance in a VPC (Virtual Private Cloud) and can be configured with one or more private IP addresses.
<li>ENIs can be moved between instances, enabling failover scenarios.
<li>You can attach multiple ENIs to an instance for high availability and load balancing.

### Example Use Case:

Attach multiple ENIs to an EC2 instance for a multi-tier application (e.g., separate network interfaces for web and database traffic).

### 4. Elastic Block Store (EBS)

Elastic Block Store (EBS) provides persistent block-level storage that can be attached to EC2 instances. Unlike instance store volumes (which are ephemeral and only exist during the instance lifecycle), EBS volumes persist even after an instance is stopped or terminated.

### Attributes:

<li>EBS volumes are highly available and durable, stored in multiple Availability Zones.
<li>EBS volumes come in different types, optimized for different use cases (e.g., SSD-backed for performance, HDD-backed for throughput).
<li>Volumes can be resized and provisioned dynamically as needed.
<li>Snapshots: EBS supports creating backups (snapshots) of volumes, which can be used to create new volumes or restore data.

### Example Use Case:

You can attach an EBS volume to an EC2 instance to store application data, databases, or file systems that need to persist beyond the instance lifecycle.

### Common EBS Volume Types:

<li>gp3 (General Purpose SSD): Balanced price/performance for most workloads.
<li>io2 (Provisioned IOPS SSD): High-performance storage for I/O-intensive applications like databases.
<li>st1 (Throughput Optimized HDD): Cost-effective storage for large, sequential workloads like log storage or big data.

### 5. Amazon Machine Image (AMI)

An Amazon Machine Image (AMI) is a pre-configured template that contains the operating system, software, and configuration settings for launching EC2 instances. AMIs allow you to quickly replicate and deploy instances with a known configuration.

### Attributes:

<li> AMIs can be public (provided by AWS or the community) or private (created by the user).
<li> An AMI consists of the root volume (e.g., an EBS volume) and the necessary configuration files for bootstrapping the instance.
<li> You can create your own custom AMIs from an existing EC2 instance, preserving the configuration and software installed.

### Example Use Case:

Create a custom AMI that includes a specific version of a web server and application software to quickly deploy consistent EC2 instances across your environment.

### 6. Elastic File System (EFS)

Amazon Elastic File System (EFS) is a scalable, fully managed file storage service that can be used with EC2 instances. It provides a simple, scalable, and highly available shared file system that can be accessed from multiple EC2 instances simultaneously.

### Attributes:

<li>EFS provides file-level storage, allowing multiple instances to read from and write to the same file system.
<li>It automatically scales as your storage needs grow, providing up to petabytes of capacity.
<li>EFS supports Network File System (NFS) v4.1 and v4.2 protocols, making it suitable for Linux-based workloads.

### Example Use Case:

Use EFS to store shared data (e.g., web server content, application logs, or home directories) across multiple EC2 instances, enabling high availability and scalability.

## High Availability & Scalability in AWS

High Availability (HA) and Scalability are critical components in cloud architecture to ensure that applications can handle varying traffic loads while minimizing downtime. AWS offers a variety of services to implement HA and scalability, including Elastic Load Balancer (ELB), Auto Scaling Groups (ASG), and multiple types of load balancers (e.g., ALB, NLB, GWLB) that distribute traffic across multiple resources and regions.

Here’s a detailed breakdown of the core components related to high availability and scalability in AWS.

### 1. Elastic Load Balancer (ELB)

Elastic Load Balancing (ELB) is a fully managed service that automatically distributes incoming application traffic across multiple targets (e.g., EC2 instances, containers, IP addresses) in one or more availability zones (AZs). ELB improves the availability, fault tolerance, and scalability of your applications.

### Types of ELBs:

<li> Application Load Balancer (ALB)
<li> Network Load Balancer (NLB)
<li> Gateway Load Balancer (GWLB)

### Key Features:

<li> Auto-scaling support: ELB works seamlessly with Auto Scaling Groups to ensure applications scale horizontally (by adding/removing instances) based on traffic patterns.
<li> Health checks: ELB performs health checks on targets (EC2 instances, containers) to ensure only healthy instances are receiving traffic.
<li> Global Reach: ELB supports traffic distribution across multiple regions and availability zones, ensuring high availability.

### 2. Sticky Sessions (Session Affinity)

Sticky sessions, or session affinity, is a feature where requests from the same client (e.g., browser or app) are routed to the same EC2 instance during the duration of a session. This is useful when an application relies on session-specific data (e.g., a shopping cart in an e-commerce website).

### How it works:

<li>The load balancer inserts a session cookie in the client’s request (usually a “AWSELB” cookie).
<li>This cookie ensures that subsequent requests from the same client will be directed to the same instance.

### Use Case: 

Sticky sessions are used in scenarios where an application maintains state between requests, such as an online shopping cart or login session, requiring the client to always be routed to the same backend.

### 3. Cross-Zone Load Balancing

Cross-Zone Load Balancing allows ELB to distribute traffic evenly across all registered targets (EC2 instances) in multiple availability zones (AZs). When enabled, ELB will balance the load across instances, regardless of the AZ they reside in.

### Benefits:

<li>Increased fault tolerance: If one AZ experiences issues, traffic can be redirected to instances in healthy AZs.
<li>Efficient resource utilization: By distributing traffic evenly across instances in all AZs, resource utilization is maximized.

### Use Case: 

If you have an application with instances spread across multiple AZs, cross-zone load balancing ensures even distribution of traffic, improving high availability and fault tolerance.

### 4. SSL (Secure Socket Layer) / TLS (Transport Layer Security)

SSL/TLS is a cryptographic protocol that secures data transmitted between clients and servers. ELB supports SSL/TLS termination, which offloads the SSL/TLS encryption and decryption from your EC2 instances, improving performance and simplifying certificate management.

### How it works:

<li>SSL Termination at ELB: ELB decrypts incoming SSL/TLS traffic and forwards unencrypted requests to backend EC2 instances.
<li>SSL Passthrough: In certain scenarios, the traffic remains encrypted between the load balancer and the backend instance, and only the client-to-ELB communication is decrypted.

### Use Case: 

For secure applications such as e-commerce sites, banking applications, or any service requiring encrypted communication between the client and the server, SSL termination at the load balancer provides efficiency and security.

### 5. Application Load Balancer (ALB)

Application Load Balancer (ALB) is a type of ELB designed to handle HTTP and HTTPS traffic. It operates at the application layer (Layer 7) and offers advanced routing features, such as host-based and path-based routing, WebSocket support, and SSL termination.

### Features:

<li>Content-based routing: Routes requests based on URL paths, hostnames, or HTTP headers. For example, routing traffic to /api to one set of EC2 instances and /images to another.
<li>SSL Termination: ALB can terminate SSL/TLS connections and route encrypted traffic to backend instances if needed.
<li>WebSockets and HTTP/2 support: ALB supports WebSockets, allowing for real-time applications, and HTTP/2, which enhances performance.
<li>Integrated with AWS WAF: ALB can integrate with AWS Web Application Firewall (WAF) to protect against common web exploits.

### Use Case: 

Ideal for web applications, microservices architectures, and APIs where content-based routing is necessary, such as directing API calls to one set of instances and static content to another.

### 6. Network Load Balancer (NLB)

Network Load Balancer (NLB) operates at the transport layer (Layer 4) and is designed to handle high-throughput, low-latency traffic, such as TCP and UDP traffic. NLB is capable of handling millions of requests per second and is ideal for applications requiring high-performance and low-latency networking.

### Features:

<li>High throughput and low latency: NLB is capable of handling sudden and unpredictable traffic spikes with minimal latency.
<li>Static IP support: NLB provides a single, static IP address per Availability Zone, which can be used for clients to connect.
<li>TLS Termination: Like ALB, NLB can terminate TLS traffic, offloading encryption/decryption from backend instances.
<li>Health checks: NLB supports health checks at the network layer, ensuring traffic is only directed to healthy instances.

### Use Case: 

Best suited for real-time applications, gaming, IoT services, or any application requiring extremely low latency and high performance, such as high-frequency trading platforms or gaming servers.

### 7. Gateway Load Balancer (GWLB)

Gateway Load Balancer (GWLB) is a load balancer designed to handle third-party virtual appliances such as firewalls, intrusion detection systems (IDS), or deep packet inspection services.

### Features:

<li>Layer 3 routing: GWLB operates at the network layer (Layer 3), making it ideal for routing traffic through third-party security services.
<li>Scalability: GWLB automatically scales with your traffic and ensures high availability by distributing traffic to virtual appliances across multiple AZs.
<li>Integration with VPC Traffic Mirroring: GWLB integrates with VPC Traffic Mirroring to capture and analyze traffic as it flows through the network.

### Use Case: 
Ideal for scenarios where you need to insert third-party security appliances (like firewalls or IDS) into the traffic flow between clients and your backend resources.

### 8. Auto Scaling Group (ASG)

An Auto Scaling Group (ASG) automatically adjusts the number of EC2 instances in response to changes in traffic load. It ensures your application scales horizontally (up or down) based on demand.

### Key Features:

<li>Automatic scaling: Based on traffic (CPU usage, request count, etc.), ASG automatically adds or removes EC2 instances.
<li>Health checks: ASGs continuously monitor EC2 instance health and replace unhealthy instances with new ones.
<li>Scaling policies: You can set up scaling policies based on CloudWatch metrics, such as scaling out when CPU usage exceeds 80% for 5 minutes.

### Use Case: 
For applications with unpredictable or fluctuating demand, ASG ensures that the right number of EC2 instances is available to handle traffic.