---
title: AWS Notes
date: 2022-10-13
tags:
  - sapling
enableToc: true
---
> Over winter break last year, I tried learning everything I could about AWS. I took a bunch of notes and decided to compile them all here. While AWS can feel overwhelming, I think it's a great tool to know for personal projects because of how cheap, fast, and battle-tested services like S3, Lambda, and Dynamo are. I hope this helps!

## Foundations of Cloud Computing

### Overview
- **Original whitepaper:** https://docs.aws.amazon.com/whitepapers/latest/aws-overview/aws-overview.pdf
- **Cloud Computing:** delivery of computing services over the internet (compute, networking, storage, analytics, development, security, databases)
- **Virtual Machine:** virtualization divides physical hardware (server) into smaller units
- usage is on a meter, pay for what you use

### Advantages
- deploy globally with regional data centers
- stop spending money running/maintaining data centers
- economies of scale through volume discounts
- increase speed and agility for quicker development
- capacity is scaled to demand
- variable expense model

### Cloud Terminology
- **High Availability:** outages are rare
- **Elasticity:** grow and shrink based on demand
- **Agility:** faster speed to market
- **Durability:** long-term data protection
- **capital expenditures:** fixed assets, operating expenses:** day-to-day operations

### Cloud Computing Models
- **Infrastructure as a Service (IaaS):** fundamental building blocks
- **Software as a Service (SaaS):** an entire app on demand
- **Platform as a Service (PaaS):** develop software with web-based tools

### Cloud Deployment Models
- **Private Cloud:** on premises, no cloud computing advantages
- **Public Cloud:** AWS advantages
- **Hybrid Cloud:** separate sensitive data from infra hosting (public + private)

### Global Infrastructure
- **Region:** grouped by geographic locations for enhanced speed
   - contains multiple AZs
   - fully independent and isolated, resources are specific to regions
- **Availability Zones (AZ):** one or more physically separated data centers with redundant power, networking, connectivity, housing
   - connected through low-latency links
   - fault tolerant
   - high availability
- **Edge Locations:** CDN to cache content for fast delivery to users with CloudFront

### AWS Account
- **AWS Management Console:** access AWS account and manage apps from your browser
- **Root User:** can do everything, protect with MFA
- **AWS CLI:** local, programmatic access (need secret key)
   - can also use SDKs for Java, Python, etc.

## Technology

### EC2
- **Elastic Compute Cloud (EC2):** rent and manage instances (virtual servers) in the cloud
   - can deploy apps directly to EC2 instances
   - use preconfigured template called Amazon Machine Image (AMI) to launch
- access through AWS Management Console, SSH, EC2 Instance Connect (EIC), or AWS Systems Manager
   - SSH is common, generate a key pair with private and public keys
- pricing
   - **On-Demand:** fixed price, billed to the second, can reserve capacity
   - **Spot:** uses unused EC2 capacity, cheap, good for interrupted workloads
   - **Reserved Instances:** commit for 1 or 3 years via contract, steady state usage
   - **Dedicated Hosts:** pay for physical server for only your instances, good for fulfilling licenses and compliance requirements
   - **Savings Plans:** commit to compute usage for 1 or 3 years, lowers bill across compute services
   - 750 free hours per month
- features
   - **Elastic Load Balancing:** distribute incoming app traffic across multiple instances (types: classic, application, gateway, network)
   - **Auto Scaling:** adds EC2 instances automatically across AZs based on demand
### Lambda
- **Lambda:** serverless compute service that lets you run code without managing servers
   - write functions in any language that scale automatically without EC2
- building block for many serverless apps, AWS manages the servers for you
- features
   - supports languages like Java, Go, Python, Ruby, Node
   - author code in your IDE
   - executes your code in response to events
   - 15-minute timeout (max execution time)
- pricing
   - **Compute Time:** time that the code takes to run
   - **Request Count:** request is counted on each execution (including tests)
   - **Always Free:** 1 million free requests per month

### Additional Compute Services
- **Fargate:** serverless compute engine for containers
   - allows you to manage containers like Docker with auto scaling
- **Lightsail:** quickly launch resources needed for small projects
   - deploy preconfigured apps like WordPress, cheap, good for inexperienced people
- **Outposts:** allows you to run cloud services in your internal data center
   - AWS delivers and installs servers in your data center to support on-premises workloads (latency, data sovereignty), hybrid experience
- **Batch:** process large workloads in batches
   - run tons of small batch processing jobs, dynamically provisions instances based on volume
- **Elastic Compute Service (ECS):** run containerized Docker apps on EC2 and Fargate
- **Elastic Kubernetes Service (EKS):** run containerized Kubernetes apps on EC2 and Fargate

### S3
- **Simple Storage Service (S3):** object storage service for the cloud that is highly available
   - objects are stored in buckets of unlimited size that can be public/private
   - can upload through console, CLI, or SDKs
- features
   - **Bucket Policies:** control access to entire buckets
   - **Access Control Lists (ACLs):** control access to individual objects
   - enable versioning to prevent accidental deletion
   - use S3 access logs to track access to buckets and objects
   - regional service, but bucket names must be globally unique
- data accessibility
   - **Durability:** objects are never lost or compromised (99.999999999%)
   - **Availability:** can access your data quickly when needed (99.99%)
- storage classes
   - **S3 Standard:** general purpose storage, low latency, supports frequent access
   - **S3 Intelligent-Tiering:** moves data to most cost effective storage class
   - **S3 Standard-Infrequent Access (IA):** less frequent, but rapid access, long-term
   - **S3 One Zone-Infrequent Access (IA):** single AZ, data can be lost
   - **S3 Glacier:** long-term backups, slow retrieval (can choose retrieval time)
   - **S3 Glacier Deep Archive:** long-term data accessed once or twice a year
   - **S3 Outposts:** object storage on-premises, good for local data or demanding apps

### Additional Storage Services
- EC2 supports storage options for instances
- **Elastic Block Store (EBS):** storage device (volume) that can be attached to your instance
   - data persists when instance is not running, one instance in same AZ
   - quickly accessible data, database on instance, or long-term storage
- **EC2 Instance Store:** local storage that is physically attached to host computer and cannot be removed
   - fast, storage is temporary since data is lost when instance is stopped
- **Elastic File System (EFS):** serverless network file system for sharing files
   - more expensive than EBS, Linux only, different AZs
   - main directories for business-critical apps or shipping apps
- **Storage Gateway:** hybrid storage service
   - connect on-premises and cloud, supports hybrid, good for cloud backups
- **Backup:** manage data backups across multiple AWS services
   - create backup plan with frequency and retention, integrates with EC2, EBS, EFS

### Content Delivery Services
- **Content Delivery Network (CDN):** delivers content quickly based on location
- **CloudFront:** CDN that delivers data and apps globally with low latency
   - makes content available globally
   - speeds up delivery of static and dynamic content
   - uses edge locations to cache content, fetches uncached content from origin
- **Global Accelerator:** sends users through AWS global network for faster delivery
   - improves latency and availability for single region apps, 60% performance boost
   - re-routes traffic to available regional endpoints

- **S3 Transfer Acceleration:** improves content uploads and downloads to/from S3 buckets
   - fast transfer of files over long distances using CloudFront’s globally distributed edge locations
   - customers around the world can upload to a central bucket

### VPC and Subcomponent
- **Networking:** connects computers and allows for data sharing securely with routers
- **Virtual Private Cloud (VPC):** foundational service that allows you to create a secure private network in AWS cloud where you launch your resources
   - private virtual network, launch resources like EC2 instances inside VPC safely
   - spans AZs in a region
- **Subnet:** allows you to split the network inside the VPC (private/public)
- **Peering:** facilitates transfer of data in a secure manner (connect 2+ VPCs)
- **Internet Gateway:** allows you to connect a VPC to the internet

### Additional Networking Services
- **Domain Name System (DNS):** directs internet traffic by connecting domain name with web servers
- **Route 53:** DNS service that routes users to applications
   - domain name registration, health checks on AWS resources, good for hybrid
- **Direct Connect:** dedicated network connection from on-premises data center to AWS
   - physical connection, data travels over private network, good for hybrid
- **Hybrid Cloud:** combination of public and private clouds
- **Site-to-Site VPN:** creates secure connection between internal networks and AWS VPCs
   - data travels over public internet (cheaper), encrypted, good for hybrid
   - **Virtual Private Gateway:** VPN connector on AWS side for VPN tunnel
   - **Customer Gateway:** VPN connector on customer side for VPN tunnel
- **API Gateway:** allows you to build and manage APIs
   - APIs let you share data between systems, can be integrated with Lambda

### Databases
- **Relational Database Service (RDS):** launch and manage relational databases
  - supports engines like MySQL, Aurora, etc.
  - high availability and fault tolerance with Multi-AZ deployment
  - AWS manages database
  - launch read replicas (read-only) for fast querying
- **Aurora:** relational database compatible with MySQL and PostgreSQL created by AWS
   - 5x faster than MySQL, 3x faster than PostgreSQL
   - scales automatically, high durability/availability
   - managed by RDS
- **DynamoDB:** fully managed NoSQL key-value and document database
   - serverless, scales automatically to massive workloads with fast performance
- **DocumentDB:** fully managed document database that supports MongoDB
- **ElastiCache:** fully managed in-memory datastore compatible with Redis and Memcached
   - data can be lost, high performance and low latency
- **Neptune:** fully managed graph database that supports highly connected datasets
   - serverless, fast and reliable

### Migration and Transfer Services
- **Database Migration Services (DMS):** helps you migrate databases to or within AWS
   - on-premises databases to AWS, continuous data replication
   - supports homogenous and heterogenous migrations without downtime
- **Server Migration Service (SMS):** migrate on-premises servers to AWS
   - server saved as new AMI, can launch servers as EC2 instances
- **Snow Family:** good for transferring large amounts of on-premises data to AWS using a physical device
   - **Snowcone:** smallest, 8 TB of storage, offline or online shipping
   - **Snowball and Snowball Edge:** petabyte-scale, transfer data in and out, cheaper than internet transfer, supports EC2 and Lambda
   - **Snowmobile:** multi-petabyte or exabyte-scale, data loaded to S3, data is securely transported with escort vehicle
- **DataSync:** allows for online data transfer from on-premises to AWS services (S3, EFS)
   - copy data over Direct Connect or internet or between AWS storage services
   - replicate data cross-region or cross-account

### Analytics Services
- **Data Warehouse:** data storage solution that aggregates massive amounts of historical data from disparate sources
   - supports querying, reporting, analytics, intelligence (not processing)
- **Redshift:** scalable data warehouse solution for non-realtime data
   - improves speed and efficiency of querying, works for exabyte-scale data
- **Analytics:** act of querying and processing data
- **Athena:** query service for S3
   - uses SQL, pay per query, serverless
- **Glue:** prepares data for analytics
   - extract, transform, load (ETL) service, better understand your data
- **Kinesis:** allows you to analyze data and video streams in real time
   - realtime streaming data, supports video, audio, app logs, clickstreams, IoT
- **Elastic MapReduce (EMR):** process large amounts of data
   - process big data, works with Hadoop and other data frameworks
- **Data Pipeline:** move data between compute/storage services on AWS or on-premises
   - moves data on specific intervals or conditions, sends success/failure notifications
- **Quicksight:** helps you visualize your data
   - interactive dashboards that can be embedded into apps

### Machine Learning Services
- **Rekognition:** automate image and video analysis with custom labels, face/text detection
- **Comprehend:** NLP service that finds relationships in text
- **Polly:** text-to-speech with natural sounding speech, many languages, and custom voice
- **SageMaker:** flagship service to build, train, deploy ML models quickly
   - prepare data, train and deploy models, deep learning AMIs
- **Translate:** realtime and batch language translation, many languages and content formats
- **Lex:** build conversational interfaces like chatbots, engaging, powers Alexa

### Developer Tools
- **Cloud9:** IDE for web browser, supports many languages, good for serverless
- **CodeCommit:** source control system for private Git repositories, Amazon’s GitHub
- **CodeBuild:** build and test source code, CI/CD, build artifacts ready for deployment
- **CodeDeploy:** manages deployment of code to compute services in cloud or on-premises
   - deploys to EC2, Fargate, Lambda, on-premises and maintains app uptime
- **CodePipeline:** automates software release process, integrates with other AWS dev tools
- **X-Ray:** debug production apps, map app components, view end-to-end requests
- **CodeStar:** helps devs collaboratively work on development projects
   - integrates with other AWS dev tools and has issue tracking dashboard (Jira)

### Deployment and Infrastructure Management Services
- **Infrastructure as Code (IAC):** write a script to provision AWS resources in a reproducible manner that saves time (like Docker)
- **CloudFormation:** provision AWS resources using IaC with templates
- **Elastic Beanstalk:** deploy your web apps and services to AWS
   - provisions resources, handles deployment, and monitors app health
- **OpsWorks:** use Chef or Puppet to automate configuration of servers and deploy code
### Messaging and Integration Services
- **Tight Coupling:** components are highly dependent on each other (monolith)
- **Loose Coupling:** components are not tightly integrated with one another (microservices)
   - queues are used to implement these
- **Simple Queue Service (SQS):** message queueing service that allows you to build loosely coupled systems
   - component-to-component communication with messages, multiple components can add to queue, messages are asynchronous
   - improves performance and scalability
- **Simple Notification Service (SNS):** send emails and text messages from your apps
   - publish messages to a topic, subscribers can receive messages
- **Simple Email Service (SES):** send richly formatted HTML emails from your apps
   - ideal for marketing or professional emails

### Auditing, Monitoring, and Logging Services
- **CloudWatch:** collection of services that help you monitor and observe cloud resources
   - collect metrics, logs, events, detect anomalies, set alarms, visualize logs
- **CloudTrail:** tracks user activity and API calls within your account
   - log and retain activity, track through console, SDKs, and CLI, see user changes and unusual activity
   - can track username, event time/name, IP address, access key, region, error code

### Additional Services
- **Amazon WorkSpaces:** host virtual desktops (Windows/Linux) on cloud, supports WFH
- **Amazon Connect:** cloud contact center service, cloud help desk

## Security and Compliance

### Shared Responsibility Model
- **Shared Responsibility Model:** public cloud has shared security responsibility between you (in the cloud) and AWS (of the cloud)
- AWS is responsible for protecting and securing their infrastructure
   - AWS global infrastructure (regions, AZs), building security (data centers), networking components (generators, AC), software
- you are responsible for how the services are implemented and managing your app data
   - app data (encryption), security configuration (API calls, rotating credentials), patching (guest OS), IAM (app security), network traffic (firewalls), software

### Well-Architected Framework
- **Well-Architected Framework:** 5 pillars that describe best practices for cloud workloads
   - **Operational Excellence:** apps that support production workloads (plan for failure, small changes, scripts, version control)
   - **Security:** put mechanisms in place to protect systems and data (automate security tasks, encryption, least privileged access, CloudTrail)
   - **Reliability:** design systems that work consistently and recover quickly (auto recovery and scaling, reduce idle resources, automate changes, test recovery)
   - **Performance Efficiency:** effective use of computing resources to meet requirements (serverless, multi-region, vendors, virtual resources)
   - **Cost Optimization:** delivering optimal and resilient solution at least cost to user (consumption-based pricing, Cloud Financial Management, measure efficiency)

### IAM Users
- **Identity and Access Management (IAM):** control access to AWS services and resources
   - secure resources, define who has access and what they can do, free service
   - **Identities:** who can access your resources (root, individuals, groups, roles)
   - **Access:** what resource they can access (policies, AWS managed policies, scope)
   - **Authentication:** present identity and provide verification
   - **Authorization:** defines which services and resources the identity has access to
- **Users:** entities created in IAM to represent the person or app accessing AWS resources
   - **Root User:** principal, close account, change email address, modify support plan
   - individual users are for everyday tasks and apps can also be users
- **Principle of Least Privilege:** give a user the minimum access required to get the job done
- **Groups:** collection of IAM users to apply common access controls to all group members

### IAM Permissions
- **Roles:** define access permissions and are temporarily assumed by an IAM user or service
- **Policies:** manage permissions for IAM users, groups, roles with a JSON policy document
- **best practices:** enable MFA, strong passwords, individual users > root, roles for EC2
- **IAM Credential Report:** lists all users in an account and status of credentials
   - has status of passwords, keys, and MFA, used for auditing and compliance
- **IAM Policy Simulator:** test and troubleshoot IAM policies

### Application Security Services
- **Firewall:** prevent unauthorized traffic to your networks by inspecting traffic against rules
- **Web Application Firewall (WAF):** protect web apps against common web attacks
   - protects against SQL injection and cross-site scripting attacks (XSS)
- **Distributed Denial of Service (DDoS):** cause traffic jam on a website to make it crash
- **Shield:** managed DDoS protection service with always-on detection
   - **Standard:** free protection against common attacks
   - **Advanced:** enhanced protection and 24/7 access to AWS experts
   - works with CloudFront, Route 53, Elastic Load Balancing, and Global Accelerator
- **Macie:** discover and protect sensitive data with ML
   - evaluates S3 environment and uncovers personally identifiable information (PII)

### Additional Security Services
- **Config:** allows you to assess, audit, and evaluate the configuration of your resources
- **GuardDuty:** uses ML to detect system threats and uncover unauthorized behavior
   - built in detection for EC2, S3, IAM and reviews CloudTrail, VPC Flow, DNS
- **Inspector:** works with EC2 instances to uncover and report vulnerabilities
- **Artifact:** offers on-demand access to AWS security and compliance reports
- **Cognito:** control access to mobile and web apps (social media sign-in)

### Data Encryption and Secrets Management Services
- **Data in Flight:** data that is moving from one location to another
- **Data at Rest:** data that is inactive and or stored for later use
- **Key Management Service (KMS):** allows you to generate and store encryption keys
- **CloudHSM:** dedicated hardware security module used to generate encryption keys
- **Secrets Manager:** allows you to manage and retrieve secrets (passwords, keys)

## Pricing, Billing, and Governance

### AWS Pricing
- pricing categories
   - **Compute:** hourly from launch to termination
   - **Storage:** data stored in the cloud
   - **Outbound Data Transfer:** data inflight moving between systems
- free offers
   - **12 Months Free:** follows initial sign-up to AWS
   - **Always Free:** do not expire
   - **Trials:** expiring trial
- pricing for common services
   - **EC2 pricing:** on-demand, savings plan, reserved instances, spot, dedicated hosts
   - **Lambda pricing:** number of requests, execution time, always free (1M requests)
   - **S3 pricing:** storage class, storage used, data transfer, request and data retrieval
   - **RDS pricing:** running clock hours, type of database, storage, purchase type, database count, API requests, deployment type, data transfer
- **Total Cost of Ownership (TCO):** financial estimate that helps you understand direct and indirect costs of AWS
   - reduce TCO by minimizing capital expenditures, utilize reserved instances, and match usage needs to provisioned resources
- **Application Discovery Service:** plan migration project to AWS (helps estimate TCO)
- **AWS Price List API:** allows you to query the price of AWS (JSON/HTML, price alerts)

### Billing Services
- **Budgets:** set custom budgets that alert you when costs exceed your budget
   - **Cost Budgets:** how much you want to spend on a service
   - **Usage Budgets:** how much you want to use a service
   - **Reservation Budgets:** RIs or Savings Plans utilization or coverage targets
- **Pricing Calculator:** get an estimate of the cost of AWS services, compare by region
- **Cost and Usage Report:** comprehensive set of cost and usage data by service category
- **Cost Explorer:** visualize and forecast costs over time
- **Cost Allocation Tags:** tags allow you to label resources using key-value pairs and track costs via the cost allocation report

### Governance Services
- **Organizations:** centrally manage multiple AWS accounts under one umbrella
   - **Master Payer:** root organization that pays entire bill
   - **Service Control Policies (SCPs):** organization policy to enforce permissions
   - **Organization Units (OUs):** grouping of AWS accounts that are similar
   - **Member Accounts:** standard AWS accounts that contain AWS resources
   - **benefits:** consolidated billing, cost savings, account governance
- **Control Tower:** ensure that accounts conform to company-wide policies
   - used for setting up new accounts, works with Organizations, dashboard
- **Systems Manager:** gives you visibility and control over AWS resources
   - automate operational tasks on resources, group resources, patch resources
- **Trusted Advisor:** real-time guidance to help you provision resources with best practices
   - **examples:** S3 permissions, MFA on root, IAM password policy, exposed keys
- **Certificate Manager:** provision and manage SSL/TLS certificates for free
- **License Manager:** helps you manage software licenses

### Management Services
- **Managed Services:** efficiently operate your AWS infrastructure
   - augments internal staff, ongoing management, reduces operational risks
- **Professional Services:** helps enterprise customers move to cloud-based operations
- **AWS Partner Network (APN):** global community of approved partners for AWS solutions
- **Marketplace:** digital catalog of prebuilt solutions you can purchase or license
   - buy third-party software, sell solutions to AWS customers, search catalog
- **Personal Health Dashboard:** alerts for events that might impact your AWS environment

### Support Plans
- support account types
   - **Basic:** included for free for all AWS accounts
   - **Developer:** $29/month, for testing and development
   - **Business:** $100/month, for production workloads, includes all Trusted Advisor checks and a Cloud Support Engineer
   - **Enterprise:** $15,000/month, for business or mission-critical production workloads, includes all Trusted Advisor checks, Technical Account Manager (TAM), and Concierge Support Team
- support case types
   - **Account and Billing:** can be opened by all customers
   - **Service Limit Increase:** can be opened by all customers
   - **Technical Support:** cannot be opened by free customers
   - does not cover debugging custom software and code development