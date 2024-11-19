# AWS Solution Architect Associate
<img src="https://images.credly.com/images/0e284c3f-5164-4b21-8660-0d84737941bc/twitter_thumb_201604_image.png" width="100" />

Learning materials for AWS Solution Architect Associate Certification (AWS-SAA-C03).

Information on the exam can be found [here](https://aws.amazon.com/certification/certified-solutions-architect-associate/).

Domains of material covered in the exam: 
* Design Resilient Architectures
* Design High-Performing Architectures
* Design Secure Architectures
* Design Cost-Optimized Architectures

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EC2.png" width="50"/> EC2 - Elastic Compute Cloud

### IP Types

Public IP - IPv4 (common) or IPv6 (IoT), can be observed anywhere in public space

Private IP - Only devices on network can see the IP address

Elastic IP - can change to work when instance is stopped and started (not rebooted)

### Placement Groups

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/cluster_placement_group.jpg" width="300"/>

#### Cluster Placement Group

Use Case: Low latency and fast, mostly for data processing and in bursts

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/spread_placement_group.jpg" width="300"/>

#### Spread Placement Group

Use Case: high availability and reliability, only 7 instances per placement group, used for continuous running applications that need to be available

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/partition_placement_group.png" width="300"/>

#### Partition Placement Group

Use Case: 7 partitions per AZ and 100s of EC2 instances, used for large distributed and replicated workloads 

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/ENI.png" width="50"/> ENI - Elastic Network Interface

<ul>
  <li>component in a VPC that represents a virtual network card</li>
  <li>implemented in same AZ as EC2 instance</li>
  <li>
    can have:
    <ol>
      <li>1 primary IPv4, one or more scondary IPv4</li>
      <li>1 elastic IP (IPv4) per private IP</li>
      <li>1 public IP</li>
      <li>1 or more security groups</li>
      <li>a MAC address</li>
    </ol>
  </li>
</ul>

### EC2 Hibernate

*stops*

### EC2 Instance Store

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/AMI.png" width="50"/> AMI Overview

Description:

Process:

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Auto-Scaling.png" width="50"/> Auto Scaling

### Auto Scaling Groups

### ASG Attributes

### ASG CloudWatch Alarms

### ASG Scaling Policies

### ASG Scaling Cooldowns

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/DynamoDB.png" width="50"/> DynamoDB

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EBS.png" width="50"/> EBS - Elastic Block Store

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EBSVolumes.png" width="50"/> EBS Volumes

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EBSSnapshots.png" width="50"/> EBS Snapshots

### EBS Snapshot Archive

### Recycle Bin for EBS Snapshots

### FSR - Fast Snapshot Restore

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EBSVolumeTypes.png" width="50"/> EBS Volume Types

### EBS Multi-Attach

### EBS Encryption

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EFS.png" width="50"/> EFS - Elastic File System

### EFS Performance and Storage Classes

### EBS vs EFS

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/ELB.png" width="50"/> ELB - Elastic Load Balancing

### High Availability and Scalability

### Load Balancing

### Health Checks

### Load Balancer Types

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/ALB.png" width="50"/> ALB - Application Load Balancer

### ALB Target Groups

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/NLB.png" width="50"/> NLB - Network Load Balancer

### NLB Target Groups

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/GLB.png" width="50"/> GLB - Gateway Load Balancer

### GLB Target Groups

### Sticky Sessions (Session Affinity)

### Sticky Sessions- Cookie Names

### ELB Cross-Zone Balancing

### SSL/TLS Bases

### SSL Certificates

### SNI - Server Name Identification

### Connection Draining

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/IAM.png" width="50"/> IAM - Identity Access Management

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Lambda.png" width="50"/> Lambda

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/RDS.png" width="50"/> RDS - Relational Database Service

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/S3.png" width="50"/> S3 - Simple Storage Service

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/SQS.png" width="50"/> SQS - Simple Queue Service

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/VPC.png" width="50"/> VPC - Virtual Private Cloud

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/CloudFront.png" width="50"/> CloudFront
