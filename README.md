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

*stop* - data on the disk of the instance is held until start

*terminate* - data on the disk is lost

*hibernate* - in-memory (RAM) is preserved, faster boot time, RAM written onto EBS volume, EBS volume must be encrypted

### EC2 Instance Store

EBS Volumes are network drives with good but limited performance. EC2 is a high-performance hardware disk-like network drive. The instance store features:
<ul>
  <li>Better I/O performance</li>
  <li>Lose storage if they are stopped (ephemeral)</li>
</ul>

Use case: Good buffer, cache, scratch data, temporary content, Risk data loss if hardware fails, backups and replications are admin's responsibility.

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/AMI.png" width="50"/> AMI Overview

Description: AMI's are a customization of an EC2 Insance. AMI's are built for specific regions and can be copied for a specific region. EC2 instances can be launched from:
<ul>
  <li>a public AMI (amazon provided)</li>
  <li>your own AMI</li>
  <li>AWS Marketplace AMS</li>
</ul>

Process:
<ol>
  <li>Start an EC2 instance and customize it.</li>
  <li>Stop the instance.</li>
  <li>Build an AMI - this will also create EBS Snapshots</li>
  <li>Laninstances from other AMI's.</li>
</ol>

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/AMIProcess.png"/>

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Auto-Scaling.png" width="50"/> Auto Scaling

### Auto Scaling Groups

### ASG Attributes

### ASG CloudWatch Alarms

### ASG Scaling Policies

### ASG Scaling Cooldowns

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/DynamoDB.png" width="50"/> DynamoDB

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EBS.png" width="50"/> EBS - Elastic Block Store

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EBSVolumes.png" width="50"/> EBS Volumes

<ul>
  <li>these are network drives that you can attach to your instances while they can.</li>
  <li>allows EC2 instances to persist data</li>
  <li>mounted to one instance at a time</li>
  <li>bound to specific availability zone</li>
  <li>free under 30GB/month</li>
</ul>

Can be detached and reattached to other instances. Capacity must be provisioned in advance (size in GB and IOPS). Volumes delete upon termination by default for root volumes and off for all other volumes.

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EBSSnapshots.png" width="50"/> EBS Snapshots

<ul>
  <li>backup of an EBS volume at a point in time</li>
  <li>not necessary to attach volume but recomended</li>
  <li>can copy snapshots across AZ's or regions</li>
</ul>

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/EBSSnapshots.png"

### EBS Snapshot Archive

Snapshots can be moved to an "archive tier" that is 75% cheaper. Takes 24 to 72 hours to restore. 

### Recycle Bin for EBS Snapshots

Can be setup with rules to retain deleted EBS Snapshots to recover from accidental deletion. Rules specify retention time (1 day - 1 year).

### FSR - Fast Snapshot Restore

Forces full initialization of snapshot to have no latency on first use, but is very expensive.

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EBSVolumeTypes.png" width="50"/> EBS Volume Types

<table>
  <head>
    <tr>
      <td>Type</td>
      <td>Description</td>
    </tr>
  </head>
  <body>
    <tr>
      <td>gp2/gp3 (SSD)</td>
      <td>General purpose SSD vlume that balances price and performance for wide variety of workloads.</td>
    </tr>
    <tr>
      <td>io1/io2 Block Express (SSD)</td>
      <td>Highest performance SSD volume for mission-critical low latency or high throughput workloads.</td>
    </tr>
    <tr>
      <td>stl (HDD)</td>
      <td>Low cost HDD volume designed for frequently accessed throughput-intense workloads.</td>
    </tr>
    <tr>
      <td>sol (HDD)</td>
      <td>Lowest cost HDD volume designed for less frequently accessed workloads.</td>
    </tr>
  </body>
</table>

Characterized by size/throughput/IOPS (I/O Operations per second). Only gp2/gp3 and io1/io2 Block Express can be used as boot volumes.

<table>
  <head>
    <tr>
      <td colspan="4"><h2>Use Cases</h2></td>
    </tr>
    <tr>
      <td colspan="2"><h4>General Purpose SSD</h4></td>
      <td colspan="2"><h4>Provisioned IOPS (PIOPS) SSD</h4></td>
    </tr>
  </head>
  <body>
    <tr>
      <td colspan="2">cost effective storage, low latency</td>
      <td colspan="2">critical business applications with sustained IOPS performance</td>
    </tr>
    <tr>
      <td colspan="2">system boot volumes, virtual desktops, development and test environments</td>
      <td colspan="2">applications that need more than 16,000 IOPS</td>
    </tr>
    <tr>
      <td colspan="2">1GiB - 16GiB</td>
      <td colspan="2">great for database workloads (sensitive to storage performance and consistency)</td>
    </tr>
    <tr>
      <td>gp3</td>
      <td>gp2</td>
      <td>io1 (4GiB - 16GiB)</td>
      <td>io2 Block Express (4GiB - 64GiB)</td>
    </tr>
    <tr>
      <td>baseline: 3000 IOPS / 125MiB/s</td>
      <td>small volumes, burst up to 3000 IOPS</td>
      <td>max PIOPS: 64000 for Nitro EC2 instances and 32000 for other</td>
      <td>sub millisecond latency</td>
    </tr>
    <tr>
      <td>max: 16000 IOPS / 1000 MiB/s independently</td>
      <td>volume and IOPS are linked: max 16000 IOPS</td>
      <td>can increase PIOPS independently from storage size</td>
      <td>max PIOPS: 256000</td>
    </tr>
    <tr>
      <td></td>
      <td>3 IOPS per GB -> means 5334GB have max IOPS</td>
      <td></td>
      <td></td>
    </tr>
  </body>
</table>

### EBS Multi-Attach - (io1/io2 family)

Attach the same EBS volume to multiple EC2 instances in the same AZ. Each instance has full read and write permissions to the high performance volume. Up to 16 EC2 instances at a time and must use a file system that is cluster aware (not XFS, EXT4, etc.).

Use case: achieve higher application availability in clusteres Linux applications that manage concurrent write operations.

### EBS Encryption

Advantages of EBS volumes:
<ul>
  <li>data at rest is encrypted indie the volume</li>
  <li>all data in-flight moving between the instance and the volume is encrypted</li>
  <li>all snapshots are encrypted</li>
  <li>all volumes created from snapshot are encrypted</li>
</ul>

Encryption and decryption are handled transparently (no admin action needed). Encryption has minimal impact on latency. EBS Encryption leverages keys from KMS (AES-256). Copying an unencrypted snapshot allows encryption. Snapshots of encrypted volumes are encrypted.

Process of encrypting an unencrypted volume:
<ol>
  <li>Create EBS Volume and EBS Snapshot of Volume.</li>
  <li>Encrypt the EBS Snapshot (using a copy).</li>
  <li>Create new EBS volume from snapshot.</li>
  <li>Now move encrypted volume to original instance.</li>
</ol>

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
