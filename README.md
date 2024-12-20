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
  <li>Launch instances from other AMI's.</li>
</ol>

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/AMIProcess.png" width="300"/>

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Auto-Scaling.png" width="50"/> Auto Scaling

### Auto Scaling Groups

Goals:
<ul>
  <li>scale out (add EC2 instances) to match an increased load</li>
  <li>scale in (remove EC2 instances) to match decreased load</li>
  <li>ensure minimum and maximum EC2 instances running</li>
  <li>automatically register as load balancer</li>
  <li>recreate unhealthy instances</li>
</ul>

### ASG Attributes

**Launch Template** - AMI + Instance type, EC2 user data, EBS volumes, security groups, SSH key pair, IAM roles for EC2 instances, network and subnet information, load balancer information

**Min/Max Size**

**Initial Capacity**

### ASG CloudWatch Alarms

Can be used to trigger ASG

### ASG Scaling Policies

<ul>
  <li>
    Dynamic Scaling
    <ul>
      <li>
        Target Tracking Scaling
        <ul>
          <li>simple setup</li>
          <li>ex: avg. ASG CPU to stay around 40%</li>
        </ul>
      </li>
      <li>
        Simple/Step Scaling
        <ul>
          <li>CloudWatch Alarm (CPU > 70%), add 2 units</li>
          <li>CloudWatch Alarm (CPU < 30%), remove 1 unit</li>
        </ul>
      </li>
    </ul>
  </li>
  <li>
    Scheduled Scaling
    <ul>
      <li>anticipate scaling based on known usage patterns</li>
    </ul>
  </li>
  <li>
    Predictive Scaling
    <ul>
      <li>control forecast and schedule scaling ahead</li>
    </ul>
  </li>
</ul>

Metrics to scale on:
<ul>
  <li>CPU Utilization: average CPU utilization across instances</li>
  <li>Request Count Per Target: stable number of requests per instance</li>
  <li>Average network In/Out</li>
  <li>Custom Metric</li>
</ul>

### ASG Scaling Cooldowns

During cooldown, there is no launching new instances or terminating instances to stabilize metrics after stabilize.

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

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/EBSSnapshots.png" width="300"/>

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

EFS is a manages NFS (Network File System) that can be mounted on many EC2 instances. Works with EC2 instances in multiple AZ's. Highliy reliable, scalable, and expensive (3x gp2) - pay per use.

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/EFS.png" width="300"/>

Use case:
<ul>
  <li>Content management, web serving, data sharing, wordpress</li>
  <li>NFSv4.1 protocol</li>
  <li>use security group to control access to EFS</li>
  <li>compatible with Linux based AMI (not Windows)</li>
  <li>encryption as rest using KMS</li>
  <li>POSIX file system</li>
  <li>file system scales automatically, billed by GB (size)</li>
</ul>

### EFS Performance Classes

<table>
  <body>
    <tr>
      <td><b>EFS Scale</b></td>
      <td>1000s of concurrent NFS clients, 10GB/s throughput</td>
    </tr>
    <tr>
      <td></td>
      <td>grow to petabyte scale network file systems, automatically</td>
    </tr>
    <tr>
      <td>Performance Mode</td>
      <td>General Purpose (default) - latency-sensitive use cases (web server, CMS, etc.)</td>
    </tr>
    <tr>
      <td></td>
      <td>Max I/O - higher latency, throughput, highly parallel (big data, media processing)</td>
    </tr>
    <tr>
      <td>Throughput Mode</td>
      <td>Bursting - 1 TB & 50MiB/s and bursts of up to 100MiB/s</td>
    </tr>
    <tr>
      <td></td>
      <td>Provisioned - set your throughput regardless of storage size (ex 1GiB for 1TB)</td>
    </tr>
    <tr>
      <td></td>
      <td>Elastic - automatically scales troughput up or down based on workloads</td>
    </tr>
  </body>
</table>

### EFS Storage Classes

Storage Tiers:
<ul>
  <li><b>standard</b> - frequently accessed files</li>
  <li><b>infrequent access (EFS IA)</b> - cost to retrieve files, lower storage price</li>
  <li><b>archive</b> - rarely accessed data</li>
  <li><b>implement life cycle policies</b> - move files between storage tiers</li>
</ul>

Availability

Standard: Multi AZ, great for production

One Zone: great for development, backup by default, compatible with 1A (EFS One Zone-1A

### EBS vs EFS

<table>
  <head>
    <tr>
      <td><b>EBS</b></td>
      <td><b>EFS</b></td>
    </tr>
  </head>
  <body>
    <tr>
      <td>one instance (except multi-attach io1/io2)</td>
      <td>share files</td>
    </tr>
    <tr>
      <td>are locked at Availability Zone level</td>
      <td>only for Linux Instances (POSIX)</td>
    </tr>
    <tr>
      <td>gp2: IO increases if disk sizes increases</td>
      <td>EFS has higher price point than EBS</td>
    </tr>
    <tr>
      <td>gp3 and io1: can increase IO increasingly</td>
      <td>can leverage storage tiers for cost savings</td>
    </tr>
  </body>
</table>

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/ELB.png" width="50"/> ELB - Elastic Load Balancing

### High Availability and Scalability

**Scalability** - ability of a web application to adapt to greater workloads

**Vertically Scalability** - increasing instance size, common for non-distributed systems

**EC2 vertical scaling** - from t2.nano (0.5GB RAM, 1 CPU) to u-12tb1.metal (12.3TB RAM, 448 CPUs)

**Horizontal Scalability** - increasing instance amount

**EC2 horizontal scaling** - Auto Scaling Group, Load Balancer

**High Availability** - Auto scaling group with multiple AZ, Load balancing with multiple AZ

### Load Balancing

These are servers that forward traffic to multiple servers downstream.

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/ELB.png" width="250"/>

Use case:
<ul>
  <li>Spread load accross multiple downstream servers</li>
  <li>Expose a single point of access (DNS) to application</li>
  <li>Seamlessly handles features downstream</li>
  <li>Do regular health checks</li>
  <li>Provide SSL termination (HTTP) for your websites</li>
  <li>Enforce stickiness with cookies</li>
  <li>Seperate public and private traffic</li>
</ul>

Elastic Load Balacer use case:
<ul>
  <li>managed</li>
  <li>lower setup costs</li>
  <li>integrated with AWS offerings/services</li>
  <ul>
    <li>EC2, EC2 Auto Scaling Group, Amazon ECS</li>
    <li>AWS Certificate Manager (ACM), CoudWatch</li>
    <li>Route 53, AWS WAF, AWS Global Accelerator</li>
  </ul>
</ul>

### Health Checks

Crusial for load balancers. They enable load balancers to know whether instances it forwards traffic to are available. Health checks are performed on port and route (/health is common).

### Load Balancer Types

**Application Load Balancer (v2)** - HTTP, HTTPS, WebSocket

**Network Load Balancer (v2)** - TCP, TLS (secure TCP), UDP

**Gateway Load Balancer** - operates at Network layer - IP Protocol

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/ALB.png" width="50"/> ALB - Application Load Balancer

Application load balancer is layer 7 (HTTP). ALB performs load balancing to multiple HTTP appliances (ex target groups) or applications (ex containers) on the same machine. Supports HTTP, HTTPS and WebSocket and redirects.

Routing tables to different target groups based on:
<ul>
  <li>path URL (example.com/<ins>users</ins> and example.com/<ins>posts</ins>)</li>
  <li>hostname in URL (<ins>one</ins>.example.com and <ins>other</ins>.example.com)</li>
  <li>query string, headers (example.com/users?<ins>id=123&order=false</ins>)</li>
</ul>

### ALB Target Groups

<ul>
  <li>EC2 instances - HTTP</li>
  <li>ECS tasks - HTTP</li>
  <li>Lambda functions - HTTP request is translated into JSON event</li>
  <li>private IP addresses</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/NLB.png" width="50"/> NLB - Network Load Balancer

Network load balancer is layer 4 (TCP). NLB forwards TCP and UDP traffic to instances. Supports millions of requests per seconds with ultra-latency. One static IP per AZ and supports assigning Elastic IPs. NLBs are used for extreme performance, TCP or UDP traffic. Not in the AWS free tier.

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/NLB.png" width="300"/>

### NLB Target Groups

<ul>
  <li>EC2 instances</li>
  <li>IP addresses (must e private)</li>
  <li>Application Load Balancer</li>
  <li>Health checks support the TCL, HTTP, and HTTPS protocols</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/GLB.png" width="50"/> GLB - Gateway Load Balancer

Deploy, scale, and manage a fleet of 3rd party network appliances in AWS (ex. Firewalls, Intrusion Detection, and Prevention systems, Deep Packet Inspection Systems, payload manipulation. Operates at laver 3 (Network layer, IP sockets). Combines transparent Network Gateway (single entry/exit for all traffic) and load balancer (distributes traffic to virtual appliances). Uses GENEVE protocol on port 6081.

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/GWLB.png" width="250"/>

### GLB Target Groups

<ul>
  <li>EC2 instances</li>
  <li>IP addresses (must e private)</li>
</ul>

### Sticky Sessions (Session Affinity)

Stickiness is directing same client to same instance behind load balancer, can be used in ALB and NLB. Cookies are used for stickiness and have controllable expiration date.

Use case: user can retain user data.

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/StickySessions.jpg" width="300"/>

### Sticky Sessions- Cookie Names

**Application-based Cookies**

Custom cookie - generated by target, can include custom atributes required by application, name specified for each target group (AWSALB, AWSALBAPP, AWSALBTG reserved by ELB)

Application cookie - generated by load balancer, name is AWSALB for ALB

**Duration-based Cookies** - generated by the load balancer, cookie name is AWSALB for ALB, AWSELB for CLB

### ELB Cross-Zone Balancing

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/CrossZoneBalancing.png" width="300"/>

Application load balancer - Cross zone balancing enabled by default, no charges

Network and Gateway Load Balancers - disabled by default, costs $

### SSL/TLS Bases

SSL certificate allows traffic between clients and load balancer to be encrypted in transit (in-flight encryption). SSL refers to secure sockets layer, used to encrypt connections. TLS referes to transport layer security (newer). TLS mainly used, some still use SSL.

Public SSL certificates are issued by Certificate Authorities. SSL certificates have an expiration date. 

### SSL Certificates

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/LoadBalancerSSL.png" width="300"/>

Load balancers use an X.509 certificate (SSL/TLS server certificate). Certificates can be managed using ACM (AWS Certificate Manager). It is possible to create and upload own certificates.

HTTPS Listeners:
<ul>
  <li>must specify a default certificate</li>
  <li>can add list of certificates to support multiple domains</li>
  <li>clients can use SNI to specify the hostname</li>
</ul>

### SNI - Server Name Identification

Solves the proble, of having multiple SSL certificates on one web server. "Newer" protocol client to indicate the hostname target server of initial SSL handshake. Server finds correct certificate, or returns default.

Only works for ALB, NLB, and CloudFront.

### Elastic Load Balancers - SSL Certificates

**Application Load Balancers** - support multiple listeners with multiple SSL certificates, ise SNI to make it work

**Network Load Balancers** - supports multiple listeners with multiple SSL certificates

### Connection Draining

Also called degredation delay, refers to time to complete "in-flight requests" while instance is deregistering or unhealthy. Stops sending new requests to the EC2 instance which is deregistering. Can be set between 1 to 3500 seconds (default 300s) or be disabled. Low values can be set if requests are short.

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/IAM.png" width="50"/> IAM - Identity Access Management

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Organizations.png" width="50"/> AWS Orgnaizations

Global service that allows management of multiple accounts.

Main account is the management account, the others are members. Members can be part of only one organization.

Allows for consolidated billing and pricing from aggregated usage. API available for automation.

Advantages:
<ul>
  <li>Multi Account vs One Account VPC</li>
  <li>Use tagging standards for billing purposes</li>
  <li>Enable CloudTrail on all accounts, send logs to central S3 account</li>
  <li>Establish Cross Account Roles for Aministration</li>
</ul>

Service Control Policies (SPC)
<ul>
  <li>IAM policies applied to OU or Accounts to restrict Users and Roles</li>
  <li>Do not apply to management account</li>
  <li>Must have expicit allow from the root through each OU in the direct path to the target account</li>
</ul>

### SCP Hiearchy

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/scp_hierarchy.png" width="300"/>

### IAM Conditions

Set conditions for applying IAM rules.

### IAM S3

Conditions can be bucket level or object level.

### Resource Policies

Principle does not give up their permissions.

### IAM Roles 

Assuming a role surrenders original permissions and takes new permissions.

### Amazon EventBridge - Security

<ul>
  <li>When a rule runs, it needs permissions on the target.</li>
  <li>Resource-based policy: Lambda, SNS, SQS, S3 buckets, API gatesway...</li>
  <li>IAM Role: Kinesis stream, EC2 Auto Scaling, Systems Manager Run Command, ECS task...</li>
</ul>

### Permission Boundaries

Permission boundaries are supported for users and roles. Advanced feature to use a managed policy to set the maximum permissions an IAM entity can get.

### Identity Center

One login for all your:
<ul>
  <li>AWS accounts in AWS Organizations</li>
  <li>Business cloud applications</li>
  <li>SAML2.0-enabled applications</li>
  <li>EC2 Windows Instances</li>
</ul>

Identity providers: Built-in identity store in IAM Identity Center

### Directory Serivces

AWS Managed Microsoft AD - create own AD in AWS, manage users locally, supports MFA; est. "trust" connections with on-premise AD

AD Connector - Directory Gateway (proxy) ro redirect to on-premise AD, supports MFA; users are managed on the on-premise AD

Simple AD - AD-compatible managed directory on AWS; cannot be joined with on-remise AD

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Control-Tower.png" width="50"/> AWS Control Tower

Easy way to setup and govern a secure and compliant multiaccount AWS environment based on best practices; uses AWS Organizations to create accounts

Benefits:
<ul>
  <li>Automate the set up of environment easily</li>
  <li>Automate ongoing policy management using guardrails</li>
  <li>Detect policy iolations and remediate</li>
  <li>Monitor compliance through an interactive dashboard</li>
</ul>

Control Tower Guardrails:
<ul>
  <li>Provides ongoing governence for Control Tower environment</li>
  <li>Preventive Guardrail - using SPCs</li>
  <li>Detective Guardrail - using AWS Config</li>
</ul>

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Lambda.png" width="50"/> Lambda

## Databases

### Database Types

**RDBMS** - RDS, Aurora - great for joins

**NoSQL** - no joins, no SQL: DynamoDB (~JSON), ElstiCache (key value pairs), Neptune (graphs), DocumentDB (for MongoDB), Kyspaces (for Apache Cassandra)

**Object Store** - S3 (for big objects), Glacier (for backups and achives)

**Data Warehouse** - RedShift (OLAP), Athena, EMR

**Search** - OpenSearch (JSON) - free text, unstructured searches

**Graphs** - Amazon Neptune - displays relationships between data

**Ledger** - Amazon Quantum Ledger Database

**Time Series** - Amazon Timestream

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/RDS.png" width="50"/> RDS Overview

Managed service for SQL databases

Create databases in the cloud:

 - Postgres, MySQL, ManiaDB, Oracle, Microsoft SQL Services, IBM DB2, Aurora

### RDS vs using DB on EC2

Advantages:
<ul>
  <li>Automatic provinsioning</li>
  <li>Continuous backups</li>
  <li>Monitoring dashboards</li>
  <li>Read replicas</li>
  <li>Multi AZ</li>
  <li>Maintanence windows</li>
  <li>Scaling (horizontal and vertical)</li>
  <li>Storage backed by EBS</li>
</ul>

Disadvantages:
<ul>
  <li>No SSH into instances</li>
</ul>

### RDS Auto Scaling

<ul>
  <li>Helps you increase storage on RDS DB instance dynamically</li>
  <li>RDS scales automattically when running out of fre storage</li>
  <li>Maximum Storage Threshold must be set</li>
  <li>Useful for applications with unpredictable workloads</li>
  <li>Supports all RDS database engines</li>
</ul>

### RDS Read Replicas

<ul>
  <li>Help scale reads from DB instance</li>
  <li>Up to 15 read replicas; within AZ, cross AZ, cross region</li>
  <li>Replications are ASYNC so reads are eventually consistent</li>
  <li>Replicas can be promoted to their own database</li>
  <li>Applications must update the connection string to leverage read replicas</li>
</ul>

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/rds_read_replicas.png" width="300"/>

Use case:
<ul>
  <li>Production DB taking a normal load</li>
  <li>Meant to run reporting that may overload DB</li>
  <li>Replicate reporting and run reporting there</li>
  <li>Read replicas are used for SELECT (not modifying with INSERT, UPDATE, DELETE)</li>
</ul>

Fees incurred on cross-region read replicas, not on cross-AZ read replicas

### RDS Multi AZ (disaster recovery)

<ul>
  <li>SYNC replication</li>
  <li>One DNS name</li>
  <li>Increase availability</li>
  <li>Failover in case of loss</li>
</ul>

### RDS Single AZ to Multi AZ

<ol>
  <li>Create snapshot of DB</li>
  <li>New DB restored from snapshot</li>
  <li>Synchronization established between the 2 DB's</li>
</ol>

### RDS Custom

<ul>
  <li>managed Oracle and Microsoft SQL Server database with OS and DB customization</li>
  <li>RDS: automates setup, operation, and scaling of DB in AWS</li>
  <li>
    Custom: access underlying database and OS to:
    <ul>
      <li>Configure settings</li>
      <li>Install patches</li>
      <li>Enable native features</li>
      <li>Access underlying EC2 instance using SSH or SSH session manager</li>
    </ul>
  </li>
  <li>deactivate automation made to perform customization, make snapshot first</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Aurora.png" width="50"/> Amazon Aurora

<ul>
  <li>Proprietary technology from AWS (not open source)</li>
  <li>Postgres and MySQL are supported on Aurora DB</li>
  <li>Aurora is cloud optimized: 5x faster than SQL, 3x faster than Postgres on RDS</li>
  <li>Storage grows automattically in increments of 10GB, up to 128 TB</li>
  <li>can have 15 replicas and replication is faster than MySQL</li>
  <li>failover is instantaneous</li>
</ul>

### Aurora High Availability and Read Scaling

<ul>
  <li>
    6 copies of data across 3 AZs
    <ul>
      <li>4 copies of 6 for writes</li>
      <li>3 copies of 6 for reads</li>
      <li>self-healing with peer-to-peer replication</li>
      <li>storage is striped across 100s of volumes</li>
    </ul>
  </li>
  <li>One Aurora instance takes writes (master)</li>
  <li>automated failover for master is 30 seconds</li>
  <li>master and up to 15 Aurora read replicas serve reads</li>
  <li>support cross region replication</li>
</ul>

### Amazon DB Cluster

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/aurora_db_cluster.png" width="300"/>

### Aurora Custom Endpoints

<ul>
  <li>define a subset of aurora instances as a custom endpoint</li>
  <li>example: run analytical queries on specific replicas</li>
  <li>reader endpoint is gernally not used after defining custom endpoints</li>
</ul>

### Aurora Services

<ul>
  <li>automated database installation and auto-scaling based on actual usage</li>
  <li>good for infrequent, intermittent, or unpredictable workloads</li>
  <li>no capacity planning needed</li>
  <li>pay per second, can be cost effective</li>
</ul>

### Global Aurora

<ul>
  <li>1 primary region (read/write)</li>
  <li>up to 15 secondary (read only) regions, replication lag is less than 1s</li>
  <li>up to 16 read replicas per secondary region</li>
  <li>decreased latency</li>
  <li>typical cross-region replication less than 1 second</li>
</ul>

### Aurora Machine Learning

<ul>
  <li>Allows ML-based predictions to applications through SQL</li>
  <li>Simple, optimized, and secure integration between Aurora and AWS ML services</li>
  <li>Supports SageMaker and Comprehend</li>
  <li>No ML experience needed</li>
</ul>

Use cases: fraud detection, ad targeting, sentiment analysis, product recomendations

### RDS Backups 

Automated Backups
<ul>
  <li>Daily full backup of the DB</li>
  <li>Transaction log backed up by RDS every 5 minutes</li>
  <li>1 to 35 days of retention, set to 0 to disable</li>
</ul>

Manual DB Snapshots
<ul>
  <li>Manually triggered</li>
  <li>Rentention of back up as long as wanted</li>
</ul>

Tip: Snapshot and restore if DB inactive for long times (Snapshot cheaper than stopped)

### Aurora Backups

Automated Backups
<ul>
  <li>1 to 35 day backups (cannot be disabled)</li>
  <li>point-in-time recovers in that timeframe</li>
</ul>

Manual Backups
<ul>
  <li>Manually triggered</li>
  <li>Retention of backup for as long as wanted</li>
</ul>

### RDS & Aurora Restore options

Restoring RDS/Aurora backup or snapshot creates a new database.

Restoring MySQL RDS from S3
<ol>
  <li>Create backup of on-premises database</li>
  <li>Store on S3</li>
  <li>Restore the backup onto a new RDS instance</li>
</ol>

Restore MySQL Aurora from S3
<ol>
  <li>Create backup of on-premises database with Percona XtraBackup</li>
  <li>Store on S3</li>
  <li>Restore the backup onto a new Aurora cluster</li>
</ol>

### Aurora Database Cloning

Create a new Aurora DB cluster from an existing one.

Faster than snapshot & restore, using copy-on-write protocal. 

Very fast and cost effective.

### RDS & Aurora Security

At-rest encryption:
<ul>
  <li>DB master and replics encryption using AWS KMS - defined at launchtime</li>
  <li>If master not encrypted, replicas cannot be encrypted</li>
  <li>To encrypt an unencrypted DB, make a snapshot and restore as encrypted.</li>
</ul>

In-flight encryption: TLS ready by default, using AWS TLS root certificates client-side-

IAM authentication: IAM roles to connect to the database

Security Groups: Control Network access to RDS / Aurora DB

No SSH except on DB Custom

Audit Logs can be enabled

### Amazon RDS Proxy

<ul>
  <li>Fully managed DB proxy for RDS</li>
  <li>Allows pooling and sharing DB connections established with DB</li>
  <li>Improve DB efficiency by reducing the stress on DB resources (ex CPU, RAM) and minimize open connections</li>
  <li>Serverless, autoscaling, and highly available</li>
  <li>Reduced RDS and Aurora failover time by 66%</li>
  <li>Supprts RDS (MySQL, PostgreSQL, MariaDB, MS SQL Server) and Aurora (MySQL, PostgreSQL)</li>
  <li>No code chage required for most apps</li>
  <li>Enforce IAM Authentication for DB and securely restore credentials in AWS Secrets Manager</li>
  <li>RDS Proxy is never publically available (accesed from VPC)</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/ElastiCache.png" width="50"/> Amazon ElastiCache

<ul>
  <li>Elasticache is used to get managed Redis or Memcached</li>
  <li>Caches are in-memory DBs with very high performance, low latency</li>
  <li>Helps reduce load off of DBs for read intesive workloads</li>
  <li>Helps make application stateless</li>
  <li>AWS takes care of OS maintanence, patching, setup configuration, monitoring, failure recovery, and backups</li>
</ul>

### ElastiCache Solution Architecture - DB Cache

<ul>
  <li>Applications query ElastiCache, if not available then from RDS adn store in Elasticache.</li>
  <li>Helps relieve load in RDS.</li>
  <li>Cache must have an invalidation strategy to ensure only current data is used.</li>
</ul>

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/elasticache_db_cache.png" width="300"/>

### ElastiCache Solution Architecture - User Session Store

<ul>
  <li>User logs into any application</li>
  <li>Application writes session data into ElastiCache</li>
  <li>User hits another instance of application</li>
  <li>The instnance retirieves the data and the user is already logged in.</li>
</ul>

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/elasticache_user_session_store.png" width="300"/>

### Elasticache - Redis vs Memcached

<table>
  <head>
    <tr>
      <td><img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/ElastiCache-for-Redis.png" width="25"/> REDIS</td>
      <td><img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/ElastiCache-for-Memcached.png" width="25"/> MEMCACHED</td>
    </tr>
  </head>
  <body>
    <tr>
      <td>Multi AZ with Auto-Failover</td>
      <td>Multi-node for partitioning of data</td>
    </tr>
    <tr>
      <td>Read replicas to scale reads and have high availability</td>
      <td>No high availability</td>
    </tr>
    <tr>
      <td>Data durability using AOF persistence</td>
      <td>Non persistent</td>
    </tr>
    <tr>
      <td>Backup and restore features</td>
      <td>Backup and restore (serverless)</td>
    </tr>
    <tr>
      <td>Supports sets and sorted sets</td>
      <td>Multi-threaded architecture</td>
    </tr>
  </body>
</table>

### Elasticache - Cache Security

<ul>
  <li>Supports IAM Authentication for Redis</li>
  <li>IAM policies on Elasticache are only used for AWS API-level security</li>
  <li>
    Redis AUTH
    <ul>
      <li>Set a "password/token" when creating Redis cluster</li>
      <li>Extra layer of security for the cache</li>
      <li>Supports SSL in-flight encryption</li>
    </ul>
  </li>
  <li>
    Memcached
    <ul>
      <li>Supports SASL-based authentication</li>
    </ul>
  </li>
</ul>

### Patterns for Elasticache

**Lazy Loading:** all the read data is cached, data can become stale in cache

**Write Through:** adds or update data in the cache when written to a DB

**Session Store:** store temporary session data in cache (using TTL features)

### Elasticache - Redis Use Case

<ul>
  <li>Gaming leaderboards are computationally complex</li>
  <li>Redis Sorted sets uarantee both uniqueness and element storing</li>
  <li>Each time a new element is added, it's ranked in real time, then added in correct order</li>
</ul>

### Ports

Important Ports:
FTP: 21
SSH: 22
SFTP: 22 (same as SSH)
HTTP: 80
HTTPS: 443

RDS Databases ports:
PostgreSQL: 5432
MySQL: 3306
Oracle RDS: 1521
MSSQL Server: 1433
MariaDB: 3306 (same as MySQL)
Aurora: 5432

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/DynamoDB.png" width="50"/> DynamoDB

<ul>
  <li>AWS proprietary, managed serverless NoSQL database, millisecond latency</li>
  <li>Capacity modes: provisioned capacitiy with optional scaling or on-demand capacity</li>
  <li>Can replace ElastiCache as key/value store</li>
  <li>Highly Available, Multi AZ by default, Read and Write are decoupled, transaction capability</li>
  <li>DAX cluster for read cache, microsecond read latency</li>
  <li>Security, authentication and authorization through IAM</li>
  <li>Event Processing: DynamoDB Streams to integrate with AWS Lambda, or Kinesis Data Streams</li>
  <li>Global Table feature: active-active setup</li>
  <li>Automated backup op to 35 days with PITR, or on-demand backups</li>
  <li>Export to S3 without using RCU within the PITR window, import from S3 without using WCU</li>
  <li>Great to rapidly evolve schemas</li>
</ul>

Use case: serverless applications development (small documents 100s KB), distrinuted serverless cache

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/DocumentDB.png" width="50"/> DocumentDB

<ul>
  <li>Aurora is AWS's implementation of PostgreSQL / MySQL</li>
  <li>Document is the same for MongoDB</li>
  <li>MongoDB is used to store, query, and index JSON data</li>
  <li>Similar deployment concepts as Aurora</li>
  <li>Fully managed, highly available and replication across 3 AZ</li>
  <li>DocumentDB storage grow automatically at 10GB increments</li>
  <li>Automatically scales to workoads with millions of requests per second</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Neptune.png" width="50"/> Neptune

<ul>
  <il>Real-time ordered sequence of every change to graph data</li>
  <il>Changes are available immediately after writing</li>
  <il>No duplicates, strict order</li>
  <il>Streams data accessible in an HTTP REST API</li>
</ul>

Use cases: send notifications when certain changes are made, maintain graph data synced in another data store, replicate data across regions

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Keyspaces.png" width="50"/> Amazon Keyspaces

<ul>
  <li>For Apache Cassandra</li>
  <li>Apache Cassandra is an open sourced NoSQL distibuted database</li>
  <li>Keyspaces is a managed Apache Cassandra-compatible database service</li>
  <li>Serverles, scalable, highly scalable, fully managed</li>
  <li>Automatically scale tables up/down based on the applications's traffic</li>
  <li>Tables are replicated 3 times across multi AZ's</li>
  <li>Uses the Cassandra Query Language (CQL)</li>
  <li>Single millisecond latency at any scale, 1000s of requests per second</li>
  <li>Capacity: on-demand mode or provisioned mode with auto scaling</li>
  <li>Encryption, backup, Point-In-Time Recovery (PITR) up to 35 days</li>
</ul>

Use cases: store IoT devices info, time-series data...

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/QLDB.png" width="50"/> Amazon QLDB

<ul>
  <li>Quantum Ledger Database</li>
  <li>A ledger is a book for financial records</li>
  <li>Fully managed, serverless, highly scalable, replication across 3 AZs</li>
  <li>Used to review history of all the changes made to your application data over time</li>
  <li>Immutable system: no entry can be removed or modified, cryptographically verifiable</li>
  <li>2-3x better performance than common ledger blockchain frameworks, manupulate data using SQL</li>
  <li>Difference with Amazon Managed Blockchain: no decentralization component, in accordance with financial regulation rules</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Timestream.png" width="50"/> Amazon Timestream

<ul>
  <li>Fully managed, fast, scalable, serverless time series database</li>
  <li>Automatically scales up or down to adjust capacity</li>
  <li>Store and analyze trillions of events per day</li>
  <li>1000s times faster and 1/10th cost of relational databases</li>
  <li>Scheduled queries, multi-measure records, SQL compatibility</li>
  <li>Data storage tiering recent data kept in memory and historical data kept in a cost-optimized storage</li>
  <li>Built-in time series analytics functions (helps identify patterns inn data in near real-time)</li>
  <li>Encryption in transit and at rest</li>
</ul>

Use cases: IoT apps, operational applications, real-time analytics...

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/S3.png" width="50"/> S3 - Simple Storage Service

### Moving Between Storage Classes

Objects can be moved through the storage classes, movement is automated through the use of Lifecycle Policies.

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/s3_storage_classes.png" width="300"/>

### Lifecycle Rules

**Transition Actions:**
<ul>
  <li>configure objects to transition to another storage class</li>
  <li>Ex: move objects to Standard 1A 60 days after creation</li>
  <li>Ex: move objects into glacier after 6 months</li>
</ul>

**Expiration Actions:**
<ul>
  <li>configure object deletion after time</li>
  <li>Ex: delete access logs after 365 days</li>
  <li>Ex: delete old versions of files (with versioning)</li>
  <li>Ex: delete multi-part uploads</li>
</ul>

*Rules can be created for a certain prefix, eg. s3://mybucket/mp3/xxx*

### Analytics

<ul>
  <li>Helps determine timeperiods for tansitioning objects into archive</li>
  <li>Recomentions for Standard and Standard-1A</li>
  <li>Report updated daily</li>
  <li>24 to 48 hours to start seeing analysis</li>
  <li>Good starting point for creating lifecycle rules, or improving them</li>
</ul>

### Requester Pays

<ul>
  <li>Bucket owners typically pay for storage and data transfer costs for bucket</li>
  <li>Requester Pays buckets bill the requester the cost of the request and download</li>
  <li>Helpful when sharing large datasets with other accounts</li>
  <li>Requester must be AWS authenticated</li>
</ul>

### Event Notifications

<ul>
  <li>notifications on s3 events</li>
  <li>typically delivered in seconds, sometimes in minutes</li>
  <li>can be set according to file type</li>
  <li>can create as many "s3 events" as desired</li>
</ul>

Can be passed to SNS, SQS, or Lambda Functions as policy checks for access.

Can be passed into Amazon EventBridge, and shared with other AWS services as destinations, allowing for advanced filtering, multiple destinations, EventBridge Capabilities.

### Baseline Performance

<ul>
  <li>Automatically scales to high request rates, latency 100-200ms</li>
  <li>Application can achieve at least 3500 PUT/COPY/POST/DELETE or 5500 GET/HEAD requests per second per prefix in a bucket</li>
  <li>No limit to number of prefixes</li>
  <li>Examples: (/folder1/sub1/, /folder1/sub2/, /1/, /2/)</li>
  <li>spreading accross 4 prefixes evenly results in 22000 requests per second for GET and HEAD</li>
</ul>

### Performance

<table>
  <head>
    <tr>
      <td>Multi-Part Upload</td>
      <td>Transfer Acceleration</td>
    </tr>
  </head>
  <body>
    <tr>
      <td>Recommended for files > 100MB</td>
      <td>Increase transfer speed by transerring file to an AWS edge location which forwards data to S3 bucket in the target region</td>
    </tr>
    <tr>
      <td>Can help parallelize uploads</td>
      <td>Compatible with multi-part upload</td>
    </tr>
  </body>
</table>

### S3 Byte-Range Fetches

Parallelize GETs by requesting specific byte ranges, has better failure resistance.

### Batch Operations

<ul>
  <li>Perform bulk operations on existing S3 objects with single request</li>
  <li>Job consists of a list of objects, the action to perform, and optional parameters</li>
  <li>manages retries, tracks progress, sends completion notifications, general reports</li>
  <li>S3 Inventory can be used to get object list and use S3 Select to filter objects</li>
</ul>

### Storage Lens

Default Dashboard:
<ul>
  <li>Visualize summarized insights and trends for both free and advanced metrics</li>
  <li>Default dashboard shows Multi-Region and Multi-Account data</li>
  <li>Preconfigured by Amazon S3</li>
  <li>Can't be deleted, but can be disabled</li>
</ul>

<h4>Metrics</h4>

**Summary**
<ul>
  <li>general insights about your s3 storage</li>
  <li>StorageBytes, ObjectCount...</li>
  <li>Use cases: identify the fastest-growing buckets and prefixes</li>
</ul>

**Cost-Optimization**
<ul>
  <li>Provide insights to manage and optimize your storage costs</li>
  <li>NonCurrentVersionStorageBytes, IncompleteMultipartUploadStorageBytes...</li>
  <li>Use cases: identify buckets with incomplete multipart uploaded older than 7 days, Identify which could be transitioned to lower-cost storage class</li>
</ul>

**Free**
<ul>
  <li>automatically available for all</li>
  <li>Contains 28 usage metrics</li>
  <li>Data is available for 14 days</li>
</ul>

**Advanced**
<ul>
  <li>additional paid metrics and features</li>
  <li>Advanced Metrics - activity, advanced cost optimization, advanced data protection, status code</li>
  <li>CloudWatch Publishing - access metrics in cloudwatch without additional charges</li>
  <li>Prefix Aggregation - collect metrics at the prefix level</li>
  <li>Data is available for queries for 15 months</li>
</ul>

### Object Encryption

SSE-S3 (Server-Side Encryption with Amazon S3-Managed keys)
<ul>
  <li>Encryption keys handled, managed, and owned by AWS</li>
  <li>Object encryption is server-side</li>
  <li>Encryption type is AES-256</li>
  <li>Enabled by default for new buckets and new objects</li>
  <li>Must set header "x-amz-server-side-encryption":"AES256"</li>
</ul>

SSE-KMS (Server-Side Encryption with KMS keys)
<ul>
  <li>Encryption keys handled, managed, and owned by AWS KMS</li>
  <li>KMS advantages: user control + audit key usage using CloudTrain</li>
  <li>Object is encrypted server side</li>
  <li>Must set header "x-amz-server-side-encryption":"aws:kms"</li>
</ul>

<ul>
  <li>SSE-KMS impacted by KMS limits</li>
  <li>Upon downloads calls the GenerateDataKey KMS API</li>
  <li>Count towards teh KMS quota per second (5500, 10000, 30000 req/s based on region)</li>
  <li>You can request a quota increase using the Service Quotas Console</li>
</ul>

DSSE-KMS (Double Server-Side Encryption with KMS keys)
<ul>
  <li>Encryption handled on double sides</li>
  <li>For customers with more rigorous security standards</li>
</ul>

SSE-C (Server-Side Encryption with Customer keys)
<ul>
  <li>Keys fully managed by customer outside of AWS</li>
  <li>S3 does not store the encryption key you provide</li>
  <li>HTTPS required</li>
  <li>Encryption key must be provided in HTTP headers, for every HTTP request made</li>
</ul>

Client-Side Encryption
<ul>
  <li>Use client libraries such as S3 Client Side Encryption Library</li>
  <li>Clients must encrypt data themselved before sending to S3</li>
  <li>Clients must decrpt data themselves when retrieving</li>
  <li>Customer fully manages the keys and encryption cycle</li>
</ul>

### Ecryption in Transit (SSL/TLS)

<ul>
  <li>Amazon S3 exposes two endpoints: HTTP (non encrypted) and HTTP (encrypted in flight)</li>
  <li>HTTPS recommended</li>
  <li>HTTPS is mandatory for SSE-C</li>
  <li>Most clients use HTTPS endpoint by default</li>
</ul>

### Default Encryption vs Bucket Policies

SSE-S3 encryption is automaticaly pplied to new objects stored in S3 bucket.

Optional: "Force Encryption" using a bucket policy and refuse any API call to PUT an S3 object without encryption headers.

### CORS

<ul>
  <li>Cross-Origin Resource Sharing</li>
  <li>Origin = scheme (protocol) + host (domain) + port</li>
  <li>Web Browser-based mechanism to allow requests to other orignins while visiting the main origin</li>
  <li>Same origin: "http://example.com/app1" and "http://example.com/app2"</li>
  <li>Different origins: "http://www.example.com" and "http://other.example.com"</li>
  <li>Requests won't be fulfilled unless the other origin allows for the requests, using CORS Headers</li>
</ul>

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/cors.png" width="300"/>

If a client makes a cross-origin request on an S3 bucket, the correct CORS headers need to enabled.

### MFA Delete

<ul>
  <li>MFA - force users to generate a code on a device (ussually mobile phone or other hardware) before doing important operations on S3</li>
  <li>MFA required to: permanently delete an object version, suspend versioning on the bucket</li>
  <li>MFA not required to: enable versioning, list deleted versions</li>
  <li>Enable versioning to use MFA delete</li>
  <li>Only bucket owner can enable/disable MFA Delete</li>
</ul>

### Access Logs

<ul>
  <li>Logging access to S3 buckets recomended for audits</li>
  <li>any request made to s3, from any account (authorized or denied) is logged into another S3 bucket</li>
  <li>Data can be analyzed using analysis tools</li>
  <li>Target loogin bucket must be in same AWS region</li>
</ul>

Log:Warnings
<ul>
  <li>do not set logging bucket to be the monitored bucket</li>
  <li>creates logging loop, bucket will grow exponentially</li>
</ul>

### Pre-Signed URLs

<ul>
  <li>Generate pre-signed URLs using the S3 console, AWS CLI or SDK</li>
  <li>
    URL Expiration
    <ul>
      <li>S3 Console - 1min up to 720mina</li>
      <li>AWS CLI - configure expiration with expires-in parameter (default 3600s - 604800s)</li>
    </ul>
  </li>
  <li>Users given pre-signed URL inherit the permissions of the user that generated the URL for GET/PUT</li>
</ul>

Examples:
<ul>
  <li>Allow only logged-in users to download a premium video from your S3 bucket</li>
  <li>Allow an ever-changing list of users to download files by generating URLs dynamically</li>
  <li>Allow temporarily a user to upload a file to a precise location in the S3 buccket</li>
</ul>

### Glacier Vault Lock

<ul>
  <li>adopt a WORM model (Write Once Read Man)</li>
  <li>Create a Vault Lock Policy</li>
  <li>Lock the policy for future edits (cannot be changed or deleted)</li>
  <li>Helpful for compliance and data retention</li>
</ul>

### Object Lock

<ul>
  <li>adopt a WORM model (Write Once Read Man)</li>
  <li>Block an object version deletion for a specified time</li>
  <li>
    Retention mode - Compliance:
    <ul>
      <li>Object versions can't be overwritten or deleted by any user, including root</li>
      <li>Object retention modes can't be changed and retention periods can't be shortened</li>
    </ul>
  </li>
  <li>
    Retention mode - Governance:
    <ul>
      <li>Most users can overwrite or delete an object version or alter its lock settings</li>
      <li>Some users have special permissions to change the retention or delete the object</li>
    </ul>
  </li>
  <li>Retention Period - protect object for a fixed period</li>
  <li>Legal hold - protect the object indefinitely, independent from retention period, can be freely placed and removed using specific IAM permission</li>
</ul>

### Access Points

Access points simplify security management for S3 buckets. Each Access Point has:
<ul>
  <li>its own DNS name</li>
  <li>ab access point policy - manage security at scale</li>
</ul>

VPC Origin:
<ul>
  <li>Possible to define access point to be accessible only from within VPC</li>
  <li>VPC Enpoint must be created to acces the Access Point (Gateway or Interface Endpoint)</li>
  <li>VPC Endpoint Policy must allow access to the target bucket and Access Point</li>
</ul>

### Object Lambda

<ul>
  <li>AWS Lambda Functions to change object before it is retrieved by the caller application</li>
  <li>Only one S3 bucket is neededm on top of which an S3 ACCESS Point and S3 Object Lambda Access Points are created</li>
</ul>

Use Cases:
<ul>
  <li>Redacting personally identifiable information for analytics or non-production environments.</li>
  <li>Converting across data formatsm such as converting XML to JSON</li>
  <li>Resizing and watermarking images on the fly using caller-specific details, such as the user who requested the object</li>
</ul>

### AWS Snow Family

Highly secure, portable devices to collect and process data at the edge, and migrate date into and out of AWS.

<table>
  <head>
    <tr>
      <td></td>
      <td></td>
      <td>Storage Capacity</td>
      <td>Migration Size</td>
      <td>DataSync Agent</td>
      <td>Storage Clustering</td>
    </tr>
  </head>
  <body>
    <tr>
      <td><img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Snowcone.png" width="25"/></td>
      <td>Snowcone</td>
      <td>8TB uable</td>
      <td>Up to 24TB, online and offline</td>
      <td>Pre-installed</td>
      <td></td>
    </tr>
    <tr>
      <td><img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Snowball-Edge.png" width="25"/></td>
      <td>Snowball Edge</td>
      <td>80TB uable</td>
      <td>Up to petabytes, offline</td>
      <td></td>
      <td>Up to 15 nodes</td>
    </tr>
    <tr>
      <td></td>
      <td>Snowmobile</td>
      <td>< 100 PB></td>
      <td>Up to exabytes, offline</td>
      <td></td>
      <td></td>
    </tr>
  </body>
</table>

Usage process:
<ol>
  <li>Request Snowball devices from the AWS console for delivery</li>
  <li>Install snowball client / AWS OpsHub on servers</li>
  <li>Connect snowball to servers and copy files with client</li>
  <li>Ship device back when data uploadd</li>
  <li>Data to be loaded into S3 bucket</li>
  <li>Snowball is completely wiped</li>
</ol>

### Edge Computing

<ul>
  <li>Processing data while its being created on an edge location.</li>
  <li>Locations may have limited internet connection and no access to computing power.</li>
  <li>Snowball Edge / Snowcone devices can be setup to do edge computing.</li>
  <li>Use cases: preprocess data, machine learning, transcoding media</li>
</ul>

### Solution Architecture: Snowball into Glacier

Snowball cannot import directly into Glacier.

Data must first use S3, then moved with an S3 lifecycle policy.

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/FSx.png" width="50"/> Amazon FSx

Launch 3rd party highperformance file systems on AWS, fully managed service.

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/FSx-for-WFS.png" width="50"/> FSx for Windows

<ul>
  <li>A fully managed Windows filesystem share drive</li>
  <li>Supports SMB protocol & Windows NTFS</li>
  <li>MS Active Directory integration, ACLs user quotas</li>
  <li>Can be mounted on Linux EC2 instances</li>
  <li>Support's MS's Distributed File System (DFS) Namespaces</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/FSx-for-Lustre.png" width="50"/> FSx for Lustre

<ul>
  <li>Lustre is a type of parallel distributed file system, for large-scale computing</li>
  <li>Lustre = Linux + cluster</li>
  <li>Machine Learning, High Performance Computing (HPC)</li>
  <li>Video Processing, Financial modeling, electronic design automation</li>
  <li>Scales up to 100s GB/s, millions of IOPS, sub-ms latencies</li>
  <li>Storage options: SSD, HDD</li>
  <li>Seamless integration with S3</li>
</ul>

### FSx File System Deployment Options

Scratch File System
<ul>
  <li>Temporary storage</li>
  <li>Data is not replicated</li>
  <li>High Burst</li>
  <li>Usage: short-term processing, optimize costs</li>
</ul>

Persistent File System
<ul>
  <li>Long-term storage</li>
  <li>Data is replicated with same AZ</li>
  <li>Rlace failed files in minutes</li>
  <li>sage: long-term processing, sensitive</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/FSx-for-NetApp-ONTAP.png" width="50"/> FSx for NetApp ONTAP

<ul>
  <li>Managed NetApp ONTAP on AWS</li>
  <li>File System compatible with NFS, SMB, iSCSI protocol</li>
  <li>Move workloads running on ONTAP or NAS to AWS</li>
  <li>Works with Linux, Windows, MacOS, VMWare Cloud on AWS, Amazon Workspaces and AppStream 2.0, EC2/ECS/EKS</li>
  <li>Storage scales up or down automatically</li>
  <li>Snapshots, replication, low-cost, compression and data de-duplication</li>
  <li>Point-in-time instantaneous cloning</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/FSx-for-OpenZFS.png" width="50"/> FSx for OpenZFS

<ul>
  <li>Managed OpenZFS file system on AWS</li>
  <li>File System compatible with NFS</li>
  <li>Move workloads running on ZFS to AWS</li>
  <li>Works with Linux, Windows, MacOS, VMWare Cloud on AWS, Amazon Workspaces and AppStream 2.0, EC2/ECS/EKS</li>
  <li>Up to 1,000,000 IOPS with < 0.5ms latency</li>
  <li>Snapshots, compression and low-cost</li>
  <li>Point-in-time instantaneous cloning</li>
</ul>

### Hybrid Cloud for Storage

<ul>
  <li>AWS is pushing for "hybrid-cloud" (part of infrastructure in the cloud, part on-premises)</li>
  <li>Causes: Long cloud migrations, security requirements, compliance requirements, IT strategy</li>
  <li>Amazon Storage Gateway exposes on-premises S3 data</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Storage-Gateway.png" width="50"/> AWS Storage Gateway

Bridge between on-premises data and cloud data.

Use cases:
<ul>
  <li>disaster recovery</li>
  <li>backup and restore</li>
  <li>tiered storage</li>
  <li>on-premises cache & low latency files access</li>
</ul>

Types of storage gateway:
<ul>
  <li>S3 File Gateway</li>
  <li>FSx File Gateway</li>
  <li>Volume Gateway</li>
  <li>Tape Gateway</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Storage-Gateway-S3.png" width="50"/> Amazon S3 File Gateway

<ul>
  <li>Configured S3 buckets are accessible using the NFS and SMB protocol</li>
  <li>Most recently used data is cached in the file gateway</li>
  <li>Supports S3 Standard, S3 Standard IA, S3 One Zone A, S3 Intelligent Tiering</li>
  <li>Transition to S3 Glacier using a Lifecycle Policy</li>
  <li>Bucket Access using IAM roles for each File Gateway</li>
  <li>SMB Protocol has integration with Active Directory (AD) for user authentication</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Storage-Gateway-FSx.png" width="50"/> Amazon FSx File Gateway

<ul>
  <li>Native access to Amazon FSx for Windows File Server</li>
  <li>Local cache for frequently accessed data</li>
  <li>Windows native compatibility</li>
  <li>Useful for group file shares and home directories</li>
</ul>

### Volume Gateway

<ul>
  <li>Block storage using iSCSI protocol backed by S3</li>
  <li>Backed by EBS Snapshot which can help restore on premises volumes</li>
  <li>Cached volumes: low latency access to most recent data</li>
  <li>Stored Volumes: entire dataset is on premise, scheduled backups to S3</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Tape-Gateway.png" width="50"/> Tape Gateway

<ul>
  <li>Some companies have backup processes using physical tapes</li>
  <li>With Tape Gateway, companies use the same processes but, in the cloud</li>
  <li>Virtual Tape Library (VTL) backed by Amazon S3 and Glacier</li>
  <li>Back up data using existing tape-based processes (and iSCSI interface)</li>
  <li>Works with leading backup software vendors</li>
</ul>

### Storage Gateway - Hardware Appliance

Alternitive to on-premises virtualization, Storage Gateway Hardware Appliances can serve the same purpose and easily purchased.

Works with File Gateway, Volume Gateway, and Tape Gateway. Has the required CPU, memory network, and SSD cache resources. Helpful for daily backups in small data centers.

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Transfer-Family.png" width="50"/> AWS Transfer Family

A fully managed service for file transfers into and out of Amazon S3 or Amazon EFS using the FTP protocol.

Supported protocols:
<ul>
  <li><img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Transfer-Family-FTP.png" width="50"/>AWS Transfer for FTP</li>
  <li><img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Transfer-Family-FTPS.png" width="50"/>AWS Transfer for FTPS</li>
  <li><img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Transfer-Family-SFTP.png" width="50"/>AWS Transfer for SFTP</li>
</ul>

Managed infrastructure, Scalable, Reliable, Highly Available (multi-AZ). Pay per provisioned endpoint per hour + data transfers in GB.

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/DataSync.png" width="50"/> AWS DataSync

Moves large amount of data from on-premises/AWS to AWS.

Can syncronize to Amazon S3, EFS, FSx.

Replication tasks can be scheduled hourly, daily, or weekly.

File permissions and metadata are preserved.

### Storage Comparison

<table>
  <head>
  </head>
  <body>
    <tr>
      <td>S3</td>
      <td>Object Storage</td>
    </tr>
    <tr>
      <td>S3 Glacier</td>
      <td>Object Archival</td>
    </tr>
    <tr>
      <td>EBS Volumes</td>
      <td>Network storage for one EC2 instance at a time</td>
    </tr>
    <tr>
      <td>Instance Storage</td>
      <td>Physical for your EC2 instance (high IOPS)</td>
    </tr>
    <tr>
      <td>EFS</td>
      <td>Network File Sytem fort Linux Instances, POSIX filesystem</td>
    </tr>
    <tr>
      <td>FSx for Windows</td>
      <td>Network File System for Windows Servers</td>
    </tr>
    <tr>
      <td>FSx for Lustre</td>
      <td>High Performance Computing Linux file system</td>
    </tr>
    <tr>
      <td>FSx for NetApp ONTAP</td>
      <td>High OS Compatibility</td>
    </tr>
    <tr>
      <td>FSx for OpenZFS</td>
      <td>Managed ZFS file system</td>
    </tr>
    <tr>
      <td>Storage Gateway</td>
      <td>S3 and FSx File Gateway, Volume Gateway (cache and stored), Tape Gateway</td>
    </tr>
    <tr>
      <td>Transfer Family</td>
      <td>FTP, FTPS, SFTP interface on top of Amazon S3 or Amazon EFS</td>
    </tr>
    <tr>
      <td>DataSync</td>
      <td>Schedule data sync from on-premises to AWS, or AWS to AWS</td>
    </tr>
    <tr>
      <td>Snowcone / Snowball / Snowmobile</td>
      <td>move large amounts of data to cloud, physically</td>
    </tr>
    <tr>
      <td>Database</td>
      <td>for specific workloads, usually with indexing and querying</td>
    </tr>
  </body>
</table>

## App Decoupling

### Messaging

**Synchronous Communications** - application to application

**Asynchronous / Event Based Communications** - application to queue to application

Decoupling applications allow for spikes in traffic. SQS, SNS, and Kinesis allow scaling independently from application.

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/SQS.png" width="50"/> Amazon SQS - Simple Queue Service 

Oldest offering, fully managed.

Attributes:
<ul>
  <li>Unlimited throughput, unlimited messages in queue</li>
  <li>Default retention of message: 4 days, max 14 days</li>
  <li>Low latency ( < 10ms on publish and recieve)</li>
  <li>Limited to 256KB per message</li>
</ul>

Messages are produced to SQS using SDK, and message is persisted in SQS until a consumer deletes it.

Messages consumers (running on EC2 instances, servers, AWS Lambda, etc.), poll SQS for messages, process messages, and delete messages.

Consumers can be arranged in parallel to handle messages, and can scale horizontally to improve throughput.

### SQS - Security

Encryption: In-flight ugin HTTPS API, At-rest using KMS keys, client-side encryption if the client prefers to encrypt/decrypt themselves.

Access Controls: IAM policies to regulate access to SQS API.

SQS Access Policies: useful for cross-account access to SQS queues, useful for allowing other services to write to queue.

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/VPC.png" width="50"/> VPC - Virtual Private Cloud

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Route-53.png" width="50"/> Route 53

### DNS

<ul>
  <li>Domain Name System translates human friendly hostnames into the machine IP addresses.</li>
  <li>DNS is the backbone of the internet, uses heirarchical naming structure: eg. .com, example.com, www.example.com, api.example.com</li>
</ul>

### DNS Terminology

<ul>
  <li>**Domain Registrar:** Amazon Route 53, GoDaddy, ...</li>
  <li>**DNS Records:** A, AAAA, CNAME, NS, ...</li>
  <li>**Zone File:** contains DNS records</li>
  <li>**Name Server:** resolves DNS queries</li>
  <li>**Top Level Domain (TLD):** .com, .us, .in, .gov, ...</li>
  <li>**Second Level Domain (SLD):** amazon.com, google.com, ...</li>
</ul>

### DNS Functionality

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/dns.jpg" width="300"/>

### Amazon Route 53

<ul>
  <li>High available, scalable, fully managed and Authoritative DNS, Authoritative: user can update the DNS records</li>
  <li>Route 53 is also a domain registrar</li>
  <li>Has the ability to check health of resources</li>
  <li>Only AWS service providing 100% availability SLA</li>
  <li>53: reference to the traditional DNS port</li>
</ul>

### Route 53 - Records

<ul>
  <li>How to the traffic should be routed for a domain</li>
  <li>
    Each record contains:
    <ul>
      <li>Domina/subdomain Name: eg example.com</li>
      <li>Record Type: eg A or AAAA</li>
      <li>Value: eg 12.34.56.78</li>
      <li>Routing Policy: how Route 53 responds to queries</li>
      <li>TTL: amount of time the record cached at DNS Resolvers</li>
    </ul>
  </li>
  <li>
    Supports the following DNS types
    <ul>
      <li>(must know) A / AAA / CNAME / NS</li>
      <li>(advanced) CAA / DS / MX / NAPTR / PTR / SOA / TXT / SPF / SRV</li>
    </ul>
  </li>
</ul>

### Route 53 - Record Types

<ul>
  <li>A: maps a hostname to IPv4</li>
  <li>AAAA: maps a hostname to IPv6</li>
  <li>
    CNAME: mapse a hostname to another hostname
    <ul>
      <li>Target is a domain name with an A or AAAA record</li>
      <li>Cant create a CNAME record for the top node of a DNS namespace (Zone Apex)</li>
      <li>eg: cant create for example.com but can for www.example.com</li>
    </ul>
  </li>
  <li>NS: Name servers for the hosted zone</li>
</ul>

### Route 53 - Hosted Zones

Public Hosted Zones - contains records that specify how to route traffic on the Internet (public domain names)

Private Hosted Zones - contains records that specify how you route traffic with one or more VPCs (private domain names)

### Records TTL (Time to Live)

**High TTL:** eg 24hr
<ul>
  <li>Less traffic on Route 53</li>
  <li>Possibly outdated records</li>
</ul>

**Low TTL:** eg 60sec
<ul>
  <li>More traffic on Route 53</li>
  <li>Records are outdated for less time</li>
  <li>Easy to change records</li>
</ul>

### CNAME vs Alias

**CNAME:** Points a hostname to any other hostname, only for non root domain

**Alias:** Points a hostname to an AWS resource, works for root domain and non root domain, free, native health checks

### Alias Records

<ul>
  <li>Maps a hostname to an AWS resource</li>
  <li>an extension to DNS functionality</li>
  <li>Automatically recognizes changes in the resource's IP adresses</li>
  <li>Unlike CNAME, can be used for the top node of a DNS namespace</li>
  <li>Alias Record is always of type A/AAAA for AWS resources</li>
  <li>TTL cannot be set</li>
</ul>

**Alias Targets:** Elastic Load Balancers, CloudFront Distributions, API Gateway, Elastic Beanstalk environments, S3 Websites, VPC Interface Endpoints, Global Accelerator accelerator, Route 53 record in the same hosted zone

### Routing Policies

<ul>
  <li>Define how Route 53 responds to DNS queries</li>
  <li>DNS does not route traffic, only responds to DNS queries</li>
</ul>

<table>
  <head>
    <tr>
      <td colspan=2>Routing Policies</td>
    </tr>
  </head>
  <body>
    <tr>
      <td>Simple</td>
      <td>
        <ul>
          <li>Typically route traffic to a single resource.</li>
          <li>Can specify multiple values in the same record.</li>
          <li>If random values are returned, a random one is chosen by the client.</li>
          <li>When Alias enabled, specify only one AWS resource.</li>
          <li>Can't be associated with Health Checks.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Weighted</td>
      <td>
        <ul>
          <li>Control the % of the requests that go to each resource, assign each record a weight.</li>
          <li>DNS records must have the same name and type.</li>
          <li>Can be associated with Health Checks.</li>
          <li>Use cases: load balancing between regions, testing new application versions...</li>
          <li>Assign a weight of 0 to record to stop traffic being directed to it</li>
          <li>If all records have weight of 0, then all records will be returned equally</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Latency-based</td>
      <td>
        <ul>
          <li>Redirect to the resource that has the least latency close to us</li>
          <li>Super helpful when latency for users is a priority</li>
          <li>Latency is based on traffic between users and AWS Regions</li>
          <li>Can be associated with health checks</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Failover</td>
      <td>
        <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/routing_policy_failover.png" width="300"/>
      </td>
    </tr>
    <tr>
      <td>Geolocation</td>
      <td>
        <ul>
          <li>Different from letency based.</li>
          <li>Routing based on user location.</li>
          <li>Specified by Continent, Country, or by US State.</li>
          <li>Should create "Default" record.</li>
          <li>Use case: website localization, restrict content distribution, load balancing,...</li>
          <li>Can be associated with health checks.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Geoproximity</td>
      <td>
        <ul>
          <li>Route traffic to resources based on geographic location of users and resources.</li>
          <li>Ability to shift more traffic to resources based on the defined bias.</li>
          <li>Bias values set size of region.</li>
          <li>Resources can be: AWS Resources (specified by AWS Region) or Non-AWS Resources (specified by latitude/longitude).</li>
          <li>Must use Route 53 Traffic Flow (advanced) to used this feature.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>IP-based</td>
      <td>
        <ul>
          <li>Routing based on clients' IP addresses.</li>
          <li>List of CIDRs provided for clients and the corresponding endpoints/locations.</li>
          <li>Use case: optimize performance, reduce network costs...</li>
          <li>Example: route end users from a particular ISP to a specific endpoint.</li>
          <li></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Multi-Value</td>
      <td>
        <ul>
          <li>Use when routing traffic to multiple resources.</li>
          <li>Route 53 return multiple values/resources.</li>
          <li>Can be associated with Health Checks (return values for healthy resources).</li>
          <li>Up to 8 healthy records are returned for each Multi-Value query.</li>
          <li>Multi-Value is not a substitute for having an ELB.</li>
        </ul>
      </td>
    </tr>
  </body>
</table>

### Health Checks

<ul>
  <li>HTTP Health Checks are only for public resources</li>
  <li>
    Health Check => Automated DNS Failover:
    <ul>
      <li>Monitor an Endpoint</li>
      <li>Monitor other Health Checks</li>
      <li>Monitor CloudWatch Alarms</li>
    </ul>
  </li>
</ul>

### Health Checks - Monitor an Endpoint

<ul>
  <li>About 15 global health checkers will check endpoint health.</li>
  <li>Health checks only pass then the endpoint responds with 2xx or 3xx codes.</li>
  <li>Health checks can be set up to pass / fail based on the text in the first 5120 bytes of the response.</li>
  <li>Configure router/firewall to allow incoming requests from Route 53 Health checkers</li>
</ul>

### Health Checks - Calculated

<ul>
  <li>Combine the results of multiple health checks into a single health check.</li>
  <li>Can use OR, AND, or NOT.</li>
  <li>Can monitor up to 256 health checks.</li>
  <li>Specify how many health checks are needed to pass.</li>
  <li>Usage: perform maintainence to website without causing all health checks to fail.</li>
</ul>

### Health Checks - Private Hosted Zones

<ul>
  <li>Route 53 health checkers are outside the VPC.</li>
  <li>They can't process private endpoints.</li>
  <li>CloudWatch Metrics can be created and associated with a CloudWatch Alarm, then a Health Check that checks the alarm itself.</li>
</ul>

### Domain Registar vs DNS Service

<ul>
  <li>Domain names can be purchased with a Domain Registrar typically by paying annual charges.</li>
  <li>The Domain Registrar provides a DNS service to manage DNS records.</li>
  <li>Other DNS servec can be selected to manage DNS records.</li>
  <li>Example: purchase the domain from GoDaddy and use Route 53 to manage DNS records.</li>
</ul>

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/CloudFront.png" width="50"/> CloudFront

<ul>
  <li>Content Delivery Network</li>
  <li>Improves read performance, content is cahced at the edge.</li>
  <li>Improves user experience</li>
  <li>216 Point of Presense globally (edge locations)</li>
  <li>DDoS Protection, integration with Shield, AWS Web Application Firewall</li>
</ul>

### Origins

**S3 Bucket:**
<ul>
  <li>For distirbuting files and caching at the edge</li>
  <li>Enhanced security with CloudFront Origin Access Control (OAC)</li>
  <li>OAC replaces Origin Access Identity (OAI)</li>
  <li>CloudFront can be used as an ingress (uploading files to S3)</li>
</ul>

**Custom Origin (HTTP):**
<ul>
  <li>Application Load Balancer</li>
  <li>EC2 instance</li>
  <li>S3 website (bucket must be enabled as static S3 website)</li>
  <li>Any HTTP backend</li>
</ul>

### CloudFront vs S3 Cross Region Replication

**CloudFront:**
<ul>
  <li>Global edge network</li>
  <li>Files are cached or a TTL (maybe delay)</li>
  <li>Great for static content that must be globally available</li>
</ul>

**S3 Cross Region Replication:**
<ul>
  <li>Must be setup in each region for replication</li>
  <li>Files are updated in near real-time</li>
  <li>Read only</li>
  <li>Great for dynamic content that needs to be available at low-latency in a few regions</li>
</ul>

### Geo Restriction

**Allowlist:** Allow users to access content only if they are in one of the countries on the aproved list

**Blocklist:** Prevent users from accessing your content if they are in one of the countries in the banned list

### Pricing

Cost of data out of CloudFront Edge location varies.

<table>
  <head>
    <tr>
      <td colspan=2>Price Classes</td>
    </tr>
  </head>
  <body>
    <tr>
      <td>Price Class All</td>
      <td>All regions - best performance</td>
    </tr>
    <tr>
      <td>Price Class 200</td>
      <td>Most regions, but not the most expensive</td>
    </tr>
    <tr>
      <td>Price Class 100</td>
      <td>Only the least expensive regions</td>
    </tr>
  </body>
</table>

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/cloudfront_price.png" width="300"/>

### Cache Invalidations

<ul>
  <li>If the back-end origin is updated, CloudFront only gets refreshed content after the TTL has expired</li>
  <li>A partial cache refresh can be forced using CloudFront invalidation</li>
  <li>All files can be invalidated (*) or only a special path (images/*)</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Global-Accelerator.png" width="50"/> AWS Global Accelerator

**Unicast IP** - one server holds one IP address

**Anycaast IP** - all servers hold the same IP address and the client is routed to the nearest one

<ul>
  <li>Leverage the AWS internal network to route to the application</li>
  <li>2 Anycast IP send traffic directly to Edge Locations</li>
  <li>Edge Locations send traffic to the application</li>
  <li>Works with Elastic IP, EC2 instances, ALB, NLB, public or private</li>
  <li>
    Consistent Performance
    <ul>
      <li>Intelligent routing to lowest latency and fast region failover</li>
      <li>No issue with client cache</li>
      <li>Internal AWS network</li>
    </ul>
  </li>
  <li>
    Health Checks
    <ul>
      <li>Global Accelerator performs health check of applications</li>
      <li>Helps make application global</li>
      <li>Great for disaster recovery</li>
    </ul>
  </li>
  <li>
    Security
    <ul>
      <li>only 2 external IP need to be whitelisted</li>
      <li>DDoS protection thanks to AWS Shield</li>
    </ul>
  </li>
</ul>

### Global Accelerator vs CloudFront

Both use AWS global network and its edge locations around the world.

Both services integrate with AWS Shield for DDoS protection.

**CloudFront**
<ul>
  <li>improves performance for both cacheable content</li>
  <li>Dynamic content (such as API acceleration and dynamic site delivery)</li>
  <li>Content served on the edge</li>
</ul>

**Global Accelerator**
<ul>
  <li>Improve performance for a wide range of applications over TCP or UDP</li>
  <li>Proxying packets at the edge to applications running in one or more AWS Regions</li>
  <li>Good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP</li>
  <li>Good for HTTP with static IP addresses</li>
  <li>Good for HTTP with required deterministic, fast regional failover</li>
</ul>

###

## AWS Containers

### Docker

Overview
<ul>
  <li>Docker is a software development platform to deploy apps.</li>
  <li>Apps are packaged into containers that can run on any platform.</li>
  <li>Apps run consistently, reguardless of where.</li>
  <li>Use cases: microservices architecture, lift-and-shift apps from on-premises to AWS cloud...</li>
</ul>

Docker Images
<ul>
  <li>Images stored in Docker repositories.</li>
  <li>Docker Hub: Public repository, find base images for many technologies</li>
  <li>Amazon ECR: Private repository, Public options</li>
</ul>

Docker Container Mangement on AWS
<ul>
  <li>Amazon Elastic Container Service (ECS)</li>
  <li>Amazon Elastic Kubernetes Service (EKS)</li>
  <li>AWS Fargate</li>
  <li>Amazon ECR</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/ECS.png" width="50"/> Amazon ECS

EC2 Launchtype
<ul>
  <li>Launch Docker containers on AWS means launching ECS tasks on ECS clusters</li>
  <li>EC2 Launch Type means provisioning and maintaining the infrastructure (EC2 instances)</li>
  <li>Each EC2 instance must rin the ECS Agent to register in the ECS Cluster</li>
  <li>AWS handles starting / stopping containers</li>
</ul>

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/ecs-ec2.png" width="300"/>

Fargate Launchtype
<ul>
  <li>Docker containers on AWS, no provisioning architecture</li>
  <li>serverless</li>
  <li>Create task definitions</li>
  <li>AWS runs ECS Tasks based on CPU/RAM requirements</li>
  <li>To scale: increase number of tasks - no more EC2 instances</li>
</ul>

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/ecs-fargate.png" width="300"/>

IAM Roles for ECS
<ul>
  <li>
    EC2 Instance Profile (EC2 Launch Tye only)
    <ul>
      <li>Used by ECS agent</li>
      <li>Makes API call to ECS service</li>
      <li>Send container logs to CloudWatch Logs</li>
      <li>Pull Docker image from ECR</li>
      <li>Reference sensitive data in Secrets Manager or SSM Parameter Store</li>
    </ul>
  </li>
  <li>
    ECS Task Role
    <ul>
      <li>Allows each task a specific role</li>
      <li>Use different roles for different ECS Services</li>
    </ul>
  </li>
</ul>

Load Balancer Integrations
<ul>
  <li>Application Load Balancer - supported and works on most use cases</li>
  <li>Network Load Balancer - recomended only for high throughput / high performance use cases, or to pair it with AWS Private Link</li>
  <li>Classic Load Balancer - supported but not recomended (no advanced features, no Fargate)</li>
</ul>

Data Volumes (EFS)
<ul>
  <li>Mount EFS file systems onto ECS tasks</li>
  <li>Works for both EC2 and Fargate launch types</li>
  <li>Tasks running in any AZ will share the same data in the EFS system</li>
  <li>Fargate + EFS = serverless</li>
  <li>Use cases: persistent multi-AZ shared storage for containers</li>
</ul>

ECS Service Auto Scaling
<ul>
  <li>Automatically increase/decrease the desired amount of ECS tasks</li>
  <li>Amazon ECS Auto Scaling uses AWS Application Auto Scaling</li>
  <li>Target Tracking - scale based on target value for a specific CloudWatch metric</li>
  <li>Step Scaling - scale based on a specified Cloudatch Alarm</li>
  <li>Scheduled Scaling - scale based on a specified date/time (predictable changes)</li>
  <li>ECS Service Autoscaling != EC2 Auto Scaling</li>
  <li>Fargate Auto Scaling is much easier to setup (serverless)</li>
</ul>

Auto Scaling with EC2 Launch Type
<ul>
  <li>Accomadates ECS Service Scaling by adding underlying EC2 Instances</li>
  <li>
    Auto Scaling Group Scaling
    <ul>
      <li>Scale ASG based on CPU Utilization</li>
      <li>Add EC2 instances over time</li>
    </ul>
  </li>
  <li>
    ECS Cluster Capacity Provider
    <ul>
      <li>Used to automatically provision and scale the infrastructure for ECS Tasks</li>
      <li>Capacity provider paired with an Auto Scaling Group</li>
      <li>Add EC2 Instances when missing capacity (CPU,RAM,...)</li>
    </ul>
  </li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/ECR.png" width="50"/> Amazon Elastic Container Registry (ECR)

<ul>
  <li>Store and manage Docker images on AWS</li>
  <li>Public and Private</li>
  <li>Fully integrated with ECS, backed by S3</li>
  <li>Access is controled through IAM</li>
  <li>Supports image vulnerability scanning, versioning, image tage, image, life cycle</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/EKS.png" width="50"/> Amazon EKS

Overview
<ul>
  <li>It is a way to launch managed Kubernetes clusters on AWS</li>
  <li>Kubernetes is an open-source system for automatic deployment, scaling and management of containerized (ussually Docker) application</li>
  <li>Alternative to ECS, similar goal but different API</li>
  <li>EKS supports EC2 if deploying worker nodes or Fargate to deploy serverless containers</li>
  <li>Use case: if company already using Kubernetes on premises or in another cloud, easier migration to AWS</li>
  <li>Kubernetes is cloud agnostic</li>
</ul>

Node Types
<ul>
  <li>
    Managed Node Groups
    <ul>
      <li>Creates and manages Nodes (EC2 instances)</li>
      <li>Nodes are a part of an ASG managed by EKS</li>
      <li>Supports On-Demand or Spot Instances</li>
    </ul>
  </li>
  <li>
    Self-Managed Nodes
    <ul>
      <li>Node created manually and registered to the EKS cluster and managed by an ASG</li>
      <li>Prebuilt AMI can be used - Amazon EKS Optimized AMI</li>
      <li>Supports On-Demand or Spot Instances</li>
    </ul>
  </li>
  <li>
    AWS Fargate
    <ul>
      <li>No maiuntainence required, no nodes managed</li>
    </ul>
  </li>
</ul>

Data Volumes - Need to specify StorageClass manifest on EKS cluster, leverage as Container Storage Interface (CSI) compliant driver.

Support for:
<ul>
  <li>Amazon EBS</li>
  <li>Amazon EFS</li>
  <li>Amazon FSx for Lustre</li>
  <li>Amazon FSx for NetApp ONTAP</li>
</ul>

### <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/App-Runner.png" width="50"/> AWS App Runner

<ul>
  <li>Fully managed service that simplifies deploying web applications and APIs at scale</li>
  <li>No infrastructure experience required</li>
  <li>Start with source code or container image</li>
  <li>Automatically builds and deploy the web app</li>
  <li>Automatic scaling, highly available, load balancer, encryption</li>
  <li>VPC access support</li>
  <li>Connect to database, cache, and message queue services</li>
  <li>Use cases: web apps, APIs, microservises, rapid production deployments</li>
</ul>

### AWS App2Container (A2C)

<ul>
  <li>CLI tool for migrating and modernizing Java adn .NET web apps into Docker Containers</li>
  <li>Lift-and-shift apps running in on-premises bare metal, virtual machines, or in any Cloud to AWS</li>
  <li>Accelerate modernization, no code changes, migrate legacy apps...</li>
  <li>Generates CloudFormation templates (compute,network,...)</li>
  <li>Register generated Docker Containers to ECR</li>
  <li>Deploy to ECS, EKS, or App Runner</li>
  <li>Supports pre-built CI/CD pipelines</li>
</ul>