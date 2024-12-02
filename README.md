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

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Lambda.png" width="50"/> Lambda

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/RDS.png" width="50"/> RDS - Relational Database Service

### RDS Overview

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

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/S3.png" width="50"/> S3 - Simple Storage Service

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/SQS.png" width="50"/> SQS - Simple Queue Service

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/VPC.png" width="50"/> VPC - Virtual Private Cloud

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/Route-53.png" width="50"/> Route 53

### DNS

<ul>
  <li>Domain Name System translates human friendly hostnames into the machine IP addresses.</li>
  <li>DNS is the backbone of the internet, uses heirarchical naming structure.</li>
</ul>

### DNS Terminology

**Domain Registrar:**

**DNS Records:**

**Zone File:**

**Name Server:**

**Top Level Domain (TLD):**

**Second Level Domain (SLD):**

### DNS Functionality

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/dns.jpg" width="300"/>

### Amazon Route 53

### Route 53 - Record Types

### Route 53 - Hosted Zones

Public Hosted Zones

Private Hosted Zones

### Public vs Private Hosted Zones

### Records TTL (Time to Live)

**High TTL:**

**Low TTL:**

### CNAME vs Alias

**CNAME:**

**Alias:**

### Alias Records

**Alias Targets:** Elastic Load Balancers, CloudFront Distributions, API Gateway, Elastic Beanstalk environments, S3 Websites, VPC Interface Endpoints, Global Accelerator accelerator, Route 53 record in the same hosted zone

### Routing Policies

<ul>
  <li>Define how Route 53 responds to DNS queries</li>
  <li>DNS does not route traffic, only responds to DNS queries</li>
  <li>
    Supports the following route policies:
    <ul>
      <li>Simple</li>
      <li>Weighted</li>
      <li>Failover</li>
      <li>Latency based</li>
      <li>Geolocation</li>
      <li>Multi-Value Answer</li>
      <li>Geoproximity</li>
    </ul>
  </li>
</ul>

### Routing Policy Simple

<ul>
  <li>Typically route traffic to a single resource.</li>
  <li>Can specify multiple values in the same record.</li>
  <li>If random values are returned, a random one is chosen by the client.</li>
  <li>When Alias enabled, specify only one AWS resource.</li>
  <li>Can't be associated with Health Checks.</li>
</ul>

### Routing Policy Weighted

<ul>
  <li>Control the % of the requests that go to each resource, assign each record a weight.</li>
  <li>DNS records must have the same name and type.</li>
  <li>Can be associated with Health Checks.</li>
  <li>Use cases: load balancing between regions, testing new application versions...</li>
  <li>Assign a weight of 0 to record to stop traffic being directed to it</li>
  <li>If all records have weight of 0, then all records will be returned equally</li>
</ul>

### Routing Policy Latency-based

<ul>
  <li>Redirect to the resource that has the least latency close to us</li>
  <li>Super helpful when latency for users is a priority</li>
  <li>Latency is based on traffic between users and AWS Regions</li>
  <li>Can be associated with health checks</li>
</ul>

### Health Checks

HTTP Health Checks are only for public resources

Health Check => Automated DNS Failover:
<ul>
  <li>Monitor an Endpoint</li>
  <li>Monitor other Health Checks</li>
  <li>Monitor CloudWatch Alarms</li>
</ul>

### Health Checks - Monitor an Endpoint

About 15 global health checkers will check endpoint health.

Health checks only pass then the endpoint responds with 2xx or 3xx codes.

Health checks can be set up to pass / fail based on the text in the first 5120 bytes of the response.

Configure router/firewall to allow incoming requests from Route 53 Health checkers

### Health Checks - Calculated

Combine the results of multiple health checks into a single health check.

Can use OR, AND, or NOT.

Can monitor up to 256 health checks.

Specify how many health checks are needed to pass.

Usage: perform maintainence to website without causing all health checks to fail.

### Health Checks - Private Hosted Zones

Route 53 health checkers are outside the VPC.

They can't process private endpoints.

CloudWatch Metrics can be created and associated with a CloudWatch Alarm, then a Health Check that checks the alarm itself.

### Routing Policy Failover

<img src="https://github.com/cgrundman/aws-saa-c03/blob/main/images/routing_policy_failover.png" width="300"/>

### Routing Policy Geolocation

Different from letency based. 

Routing based on user location.

Specified by Continent, Country, or by US State.

Should create "Default" record.

Use case: website localization, restrict content distribution, load balancing,...

Can be associated with health checks.

### Routing Policy Geoproximity

Route traffic to resources based on geographic location of users and resources.

Ability to shift more traffic to resources based on the defined bias.

Bias values set size of region.

Resources can be: AWS Resources (specified by AWS Region) or Non-AWS Resources (specified by latitude/longitude).

Must use Route 53 Traffic Flow (advanced) to used this feature.

### Routing Policy IP-based

Ruoting based on clients' IP addresses.

List of CIDRs provided for clients and the corresponding endpoints/locations.

Use case: optimize performance, reduce network costs...

Example: route end users from a particular ISP to a specific endpoint.

### Routing Policy Multi-Value

Use when routing traffic to multiple resources.

Route 53 return multiple values/resources.

Can be associated with Health Checks (return values for healthy resources).

Up to 8 healthy records are returned for each Multi-Value query.

Multi-Value is not a substitute for having an ELB.

### Domain Registar vs DNS Service

Domain names can be purchased with a Domain Registrar typically by paying annual charges.

The Domain Registrar provides a DNS service to manage DNS records.

Other DNS servec can be selected to manage DNS records.

Example: purchase the domain from GoDaddy and use Route 53 to manage DNS records.

## <img src="https://github.com/cgrundman/aws-saa-c03/blob/main/icons/CloudFront.png" width="50"/> CloudFront
