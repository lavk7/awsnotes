# Notes AWS
## EFS 
- Amazon EFS is designed to be highly durable and highly available
- no minimum fee or setup costs
-  concurrently-accessible storage for up to thousands of Amazon EC2 instances.
-  Amazon EFS uses the NFSv4.1 protocol
-   compatible with all Linux-based AMIs for Amazon EC2
-   Throughput scale	EFS :10+ GB per second 	EBS: Up to 2 GB per second.
-   Use case: Big data and analytics, media processing workflows, content management, web serving, and home directories.	


## VPC
- Control:
    -  selection of your own IP address ranges
    -  creation of subnets
    -  and configuration of route tables and network gateways
- VPC endpoints
    - Gateway type endpoints are available only for AWS services including S3 and DynamoDB.
    - Interface type endpoint
- Billing:
    - There are no additional charges for creating and using the VPC itself. Usage charges for other Amazon Web Services, including Amazon EC2,
    - Data transfer charges are not incurred when accessing Amazon Web Services, such as Amazon S3, via your VPC’s Internet gateway.
- IGW:
    -  horizontally-scaled, redundant, and highly available. It imposes no bandwidth constraints.
- Internet connectivity in Private subnetg:
    - Nat gateway/instance
    - Direct connect or VPN 
- VPN 
    - Support IPsec - AUthenticating and Encryping internet packets
    - Static and dynamic(needs BGP)
- Connect a VPC to my corporate datacenter:
    - Hardware VPN connection between your existing network and Amazon VPC
    - NAT not done by AWS
- Subnet 
    - Amazon VPC supports five (5) IP address ranges, one (1) primary and four (4) secondary for IPv4. Each of these ranges can be between /28 (in CIDR notation) and /16 in size.
    - Default VPCs are assigned a CIDR range of 172.31.0.0/16.
    - 200 subnets per vpc (contact support center
    - Amazon reserves the first four (4) IP addresses and the last one (1) IP address of every subnet for IP networking 
    - Private ip address remains active till termination
    - Each subnet is associated with specific AZ
    - $0.01 per GB within different AZ
- Multicast and broadcast not supported
- Security
    - Security group and ACL
- Security Group:
    - Security group operates at instance level
    - Stateful
        - What comes in can go out.
    - Only contains allow rules
    - inbound/outbound rules
- ACL:
    - Operate at subnet level
    - Stateless:
        - Both inbound and outbound rules must be created
    - Does not operate at communication within subnet
    - Contain both allow and deny
- Mode of communication between vpc:
    -  Inter-Region VPC Peering, 
    -  public IP addresses,
    -   NAT gateway, 
    -   NAT instances, 
    -   VPN Connections 
    -   Direct Connect connections.
- Network traffic monitor: AWS VPC flow logs
- VPC and EC2:
    - initially limited to launching 20 Amazon EC2
    - maximum VPC size of /16 (65,536 IPs)
    - instance launched in a VPC using an Amazon EBS-backed AMI maintains the same IP address when stopped and restarted.
- ENI:
    - Multiple ENI can be attached to same instance
    - The max depends on instance type
    - Within same AZ
    - Cannot detach primary interface
- Peering
    - Cannot use AWS Direct Connect or hardware VPN connections to access peered VPCs
    - Does not require IGW 
    - Inter region vpc peering encrypted with AEAD
    - Ipv6 no supported
    - AWS services not supported: Network Load Balancers, AWS PrivateLink and EFS
- ASN
    -  Private Autonomous System Number(ASN) allows customers to set the ASN on the Amazon side of the BGP session for VPNs and AWS Direct Connect private VIFs 
- Private Link
    - Access services powered by PrivateLink from their VPC or their on-premises, without using public IPs
    - enabling the private connectivity between AWS services using Elastic Network Interfaces (ENI) with private IPs in your VPCs

### VPC Default Limits
- Five Amazon VPCs per AWS account per region
- Two hundred subnets per Amazon VPC
- Five Amazon VPC Elastic IP addresses per AWS account per region
- One Internet gateway per VPC
- Five virtual private gateways per AWS account per region
- Fifty customer gateways per AWS account per region
- Ten IPsec VPN Connections per virtual private gateway


## S3
- durable, highly available, and infinitely scalable data storage  with low costs
- object storage 
- limit PUT is 5 gigabytes
- Support multipart
- Classes:
    - Standard: 99.999999999 (Nine 9s)% durabilty
    - Standard Infrequent Access (IA) : lower price compared to standard
    - Standard One Zone IA: not reslient, 99.5 % availability
    - Glacier: lowcost, for archive
- Key object with object tagging
- S3 encryption:
    - SSE-S3: Aws manage keys
    - SSE-C: Customer manage keys
    - SSE-KMS: Manage keys by KMS
- Lifecycle policies allow to tier objects from S3 Standard-IA to S3 One Zone-IA or Amazon Glacier. 
- In Place query
    -  query this data in place on Amazon S3
    -  S3 Select, Amazon Athena, and Amazon Redshift Spectrum
    -  S3 Select:
        -  simple SQL expressions
        -  CSV, JSON, or Apache Parquet format
        -  Compression : GZIP, BZIP2
        -  Support SELECT and WHERE
    -  Athena:
        -  Serverless
        -  Supports more complex queries like joins
        -  CSV, JSON, or Apache Parquet format
        -  CSV, JSON, ORC, Apache Parquet and Avro.
    -  Redshift Spectrum
        -  Seperate storage and compute
        -  Queries against exabytes of data in s3
        -  scales out to thousands of instances if needed, so queries run quickly regardless of data size
        -  Best for adhoc queries required for analysis
-   S3 Transfer Acceleration :
    -   enables fast, easy, and secure transfers of files over long distances between your client and your Amazon S3 buck
    -   If very large file, use s3 TA, else if less than 1G, use Cloudfront
-   Metrics
    -   CloudWatch Request Metrics will be available in CloudWatch within 15 minutes after they are enabled
    -    CloudWatch Storage Metrics are enabled by default for all buckets, and reported once per day.
-    Ipv6 is supported


## Dynamodb
- Fully managed NoSQL database 
- offers encryption at rest
- scale up or scale down your tables' throughput capacity without downtime or performance degradation
- provides on-demand backup capabili
- data is stored on SSD and automatically replicated across multiple Availability Zones
- global tables to keep DynamoDB tables in sync across AWS Regions
- SDK 
    - Interfaces: Low Level, Document , Object Persistance 
- Throughput settings:
    - specify its provisioned throughput capacity. DynamoDB uses this information to reserve sufficient system resources
    - Optionally use autoscaling and let DynamoDb decide
    - Read Capacity unit represents one strongly consistent read per second, or two eventually consistent reads per second for 4kb
    - A write capacity unit represents one write per second, for an item up to 1 KB in size.
    - For excess in Read and Write Capacity units, DynamoDb will throttle the request and return exception
    - DynamoDb can use Burst capcaity to accomodate these excess units
- Considerations in throughput settings:
    - Item sizes
    - Expected read and write request rates.
    - Read consistency requirements
- Autoscaling
    - As soon as capacity exceeds the requrirment, cloudwatch triggers and alarm
    - Notification sent to user
    - Cloudwatch alarm invokes update table command
- The Query operation finds items based on primary key values. 
- A Scan operation reads every item in a table or a secondary index. 
- Seconddary Index:
    - Secondary index is an alternative key to support queries on an item apart from primary key
    - Associated with only one table called base table.
    - Attributes can be projected or copied from base table
    - Consist of partition key and sort key
    - Two types
        - Local SI: Same PK but different SK  compared to basetable
        - Global SI: Different PK and SK
- Streams:
    -  captures a time-ordered sequence of item-level modifications in any DynamoDB table, and stores this information in a log for up to 24 hours
    -  ordered flow of information about changes to items in an Amazon DynamoDB table
    -  Each stream record appears exactly once in the stream.
    - Same sequence
    - AWS maintains separate endpoints for DynamoDB and DynamoDB Streams. 
- Backup and Recover:
    -  you can restore that table to any point in time during the last 35 day
- GLobal table:
    - deploying a multi-region, multi-master database, without having to build and maintain your own replication solution. 
- Encryption at rest is done throught AWS KMS
- DynamoDB Accelerator (DAX): delivers fast response times for accessing eventually consistent data.
    - As an in-memory cache, DAX reduces the response times of eventually-consistent read workloads by an order of magnitude, from single-digit milliseconds to microseconds.
    - for read-heavy and bursty workloads
    - Ideal for 
        - Fastest possible read time ( gaming site, trading, etc)
        - Need small items more frequently ( example sale )
        - Read intensive and cost sensitive data
        - Divert long running read intensive applications to DAX
- Limits
    - Table per account: 256 per region (Initial)
    - SI : 5 LSI and 5 GSI, Total number of projected attributes = 20
    - Max length of partition key 2048 bytes
    - Sort key - 1024 bytes


## ELB
- Types:
    - Application LB: Flexble application management and TLS termination
    - Network LB: High performance and static ip
    - Classic LB: For EC2 classic network
- Access ELB without using public ip via AWS privatelink
- Adding AZ, create an LB node and Network Interface in that AZ
- When cross zone lb is enabled, load is distributed to all the targets in all zone



## Application Load Balancer:
- HTTP, HTTPS
- WebSockets and Secure WebSockets support 
- Request tracing supported
- use security group to configure frontend of ALB
- Works on application layer , 7th layer
- NEed to setup listner, which listen to the request by clients and forward it to target groups defined by the rules of the listner
- Cloudtrail to get history of api calls
- Support for path-based routing. example : /img/*, /service/
- Host based routing
- Routing to same instance supported
- Containerized applications
- Ip address outside vpc
- Cross Zone enabled by default
- Algorithm: Round robin independent of each target group
### Listner
-  A listener is a process that checks for connection requests, using the protocol and port that you configure
-  Configuration:
    -  Protocol - HTTP,HTTPS, Port: 1-65535
    -  Offload decryption to load balancer side instead of application side
    -  Must deploy exactly one SSL cert
-  Rules:
    -  Must have one default rule without condition, basically fallback rule
    -  Rule priority is from smallest to highest, default being last
    -  Rule actions - Congnito,authentication-oidc,fixed-response,forward,redirect
- Condtions:
    - Host: Each rule can have upto 1 host condition
        - forward requests based on host name in the host header
    - Path: Each rule can have upto 1 path condition
        - Forward based on url of the request
- Rule Actions:
    - Redirect:
        - Redirect to another URL
        - Can be HTTP -> HTTPS, HTTP->HTTP, HTTPS-> HTTPS
        - 301 ( temperary ) and 302 ( permenant ) configurable
        - format `protocol://hostname:port/path?query`
        - Variables can be reused like #{protocol}
    - Fixed action responses:
        - Return 2XX,4XX,5XX responses
- Forward:
    - forward requests to specific target grup

### Target Groups
- HTTP/HTTPS, 1-65535
- Target type : Instance or IP (or ASG)
- Attributes:
    - Deregistration Delay:
        - The time after which target will be deregistered
        - default is 300s
    - Slow start
        - The duration after which LB will start redirecting traffic
        - default is 0
    - Sticky session
        - If enabled, the LB will redirect traffict from same client to the same target in the target group
        - Needs cookie enabled
- Monitor:
    - Access logs
    - Cloudwatch Metrics
    - Request tracing
    - Cloudtrail logs

## NLB
- Operates on 4th layer of OSI
- Much faster than ALB
- Supports cross zone load balancing
- single point of contact for client
- Ability to handle volatile worklaods and millions of request per second
- Support for static ip for load balancer
- Support for multiple ports on the same instance
- flow hash algorightm to redirect the traffic
- Attributes
    - Deletion protction
    - cross zone balancing

### Listeners
- TCP, 1-65535
- Listener rules almost sam as ALB

### Targets
- Routing configuration, TCP, 1-65535
- Target TYpe: Instance of IP
- Atrributes:
    - Deregistration delay: Default 300 sec ( max 3600 sec ). initial state being draining state
    - proxy_protocol_v2: Send additional contents such as Source and destination in header

### Monitoring
- Cloudwatch metrics
- VPC flow logs
- Cloudtrail logs


### Limits
#### Regional Limits

- Network Load Balancers per region: 20

- Target groups per region: 3000 *

#### Initial Load Balancer Limits 

- Listeners per load balancer: 50

- Subnets per Availability Zone per load balancer: 1

- [Cross-zone load balancing disabled] Targets per Availability Zone per load balancer: 500

- [Cross-zone load balancing enabled] Targets per load balancer: 500

- Load balancers per target group: 1


## Classic LB
- A load balancer distributes incoming application traffic across multiple EC2 instances in multiple Availability Zones.
- Support for EC2-Classic
- Support for TCP and SSL/TLS, HTTP, HTTPS
- Internet Facing and Internal
- Two types:
    - Internet facing:
        - publicly resolvable DNS name to the public ip address
        - Have public ip address
        - `name-1234567890.region.elb.amazonaws.com`
    - Internal
        - Have private ip address
        - publicly resolvable to the private ip address of the nodes
        - `internal-name-123456789.region.elb.amazonaws.com
- Subnet with atleast 8 free ips required in one AZ
- SG and ACL should allow listnere port and health check port
- Protocols
    - TCP/SSL: 
        - If tcp is used for front end and backend of LB, then headers are not modified
        - For backend, source = lb ip address
        - Enable proxy protocol to see the ip of actual source
        - Parse first line to see source info
        - Layer 4
    - HTTP/HTTPS:
        - Operates on Layer 7
        - Terminates the connection before sending to Backend
        - Consist of header X-forwarded-for which contain ip address of client
        - Can enable sticky sessions
        - Option to enable keep-alive to reuse the same connection
    - HTTPS/SSL
        - SSL termination or SSL offloading happens on loadbalancer
        - need to install x.509 certificate on LB


## CloudFront
- Distribute content with low latency and High Data Transfer
- Speed up serving static and dynamic content such as .js, .img,.css
- Delivered throught Worldwide data centers called edge location
- When user request, his request is redirected to the edge location with lowest latency
- Delivered immediately if CF already has content
- If not, CF retrieves it from source like S3 and then deliver
- Configuration:
    - Content Origin: S3,HTTP, etc
    - Access: Restrict users
    - Security: Use HTTPS
    - Cookie or Query Strings: Foward cookie or query strings to origin
    - Geo Restrictions
    - Access Logs: Create access logs or not
- Support:
    - Static and dynamic content over HTTP or HTTPS
    - Video on demand
    - Live event like streaming conferance
    - Does not support Adobe flash distribution over HTTP or HTTS. Rather use CF RMPT
    - Default Expiration period is 24 hrs
    - Use invalidation api to remove potentially harmful content taken out immediately
    - Invalidation limit: 3000 objects
    - GET, HEAD, POST, PUT, PATCH, DELETE and OPTIONS requests. 
    - POST, PUT, DELETE, and PATCH: No caching
    - Field Level Encryption:
        - Securely upload sensitive information like Credit card
        - Further encrypts the data over HTTPS using keys provided before PUT/POST request is forwarded to origin
        - Ensures that data can be decrypted by certain part of your application stack
    - Dedicate IP Custom SSL:
        - Allocated dedicated IP to server SSL content. One to one mapping with IP and SSL Cert. 
        - Older browsers
    - SNI custom SSL:
        - Allow many certifcates to be served on a single ip ussing SNI extenstion of TSL
        - Newer browsers
    - Integration with AWS Certificate Manager ( ACM ). Can deploy certificates to CF Distrubution with few clicks
    - Support for private and paid contents
    - AWS WAF:
        - Acts as a web firewall that helps protect web based on IP, Http Headers, and URI strings
    - Support gzip compression
- Streaming:
    - Protocols:
        - HTTP Live Streaming (HLS)
        - MPEG Dynamic Adaptive Streaming over HTTP (MPEG-DASH)
        - Adobe’s HTTP Dynamic Streaming (HDS) 
        - Microsoft’s Smooth Streaming
    - VOD supported
    - AWS elemental MediaConvert to supported formates (HLS, MPEG-DASH, HDS) and this can be directly streamed over CDN without using any media server
    - AWS Elemental MediaPackage: Allow multiple delivery
    - AWS Elemental MediaStore: High performance storage for Media files
- Limits:
    - 40 Gbps or 100,000 RPS
    - Web Distribution: 200
    - RTMP Dist: 100
    - CNAME's: 100
    - https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_cloudfront
- Lambda@edge
    - Run code at global edge location 
    - Reduce network latency

## EBS:
- Block level storage
- Highly Available:
    - EBS volume is replicated within same AZ 
    - Able to attach to instance within same AZ
    - Multiple volumes can be attached to same instance but one volume cannot be attached to multiple instance
- Data persistance:
    - Data exists independent of lifecylce of instance
    - Volume attached throught start-stop cycle
    - Data persist untill volume is deleted
- Data encryption:
    - All volumes support encryption
    - Uses AES 256 algorithms
    - Uses AWS KMS
    - Encryption occurs to data in transit throught the attached EC2 instance
- EBS Snapshots:
    - Backup of volume stored redundantly in S3
    - Encrypted volumes always creat encrypted snapshots and vice versa
    - Snapshot can encrypted during copying
    - Snapshots are incremental, only the block that is changed is backed up
- Volume can be modified without detaching on Current generation EC2 for non-root volume
- To OS, volume appears as Physical HD of 512 Bytes sectors
- Partition schemes supported: MBR (2^32 - 1 address blocks) GPT ( 2^64 Address blocks)


### EBS volume types:
- Terms
    - Baseline performance is the IOPS at which volume operates without burst
    - Burst is the extra IOPS provisioned when more IO happens
    - Initial Credit balance is balance of I/O which gets depleted when using burst
    - Throughput = Volume size * I/O Size (in KiB) * I/O rate
- General Purpose SSD (gp2):
    - 1GiB-16Tib
    - 100 IOPS - 10000 IOPS ( 3 IOPS/GiB)
    - One I/O size limit- 256KiB/s
    - Max through put - 160MiB/s (min size = 214GiB, from T=VIR)
    - Burst@3000 IOPS
    - Initial credit balance - 5.4 Million IOC
    - Baseline performance = 3*GiB
    - Burst IOPS is used when more IO operations are needed
    - Initial credit balance is used to calculate the max duration of burst
    - For GiB > 1000, Burst duration is infinite
    - Test and devlopment environments
- Provisioned IOPS SSD (io1):
    - 4GiB-16TiB
    - Does not use credit burst model
    - User specifiy the specify a consistent IOPS rate.
    - Throughput limit of 1 IO - 256 KiB/s
    - Max IOps - 32000  IOPs
    - Throughput limit - 500 MiB/s
    - Best for databases
- Throughput Optimized HDD (st1):
    - Dominant performance attribute: MiB/s
    - 500 - 16 TiB
    - Baseline = 40 * (TiB) = 40 MiB/s for 1 TiB
    - Burst @250MiB/TiB
    - Baseline limit - 500 MiB/s
    - Burst limit - 500 MiB/s
    - Credit capacity - 1 TiB
    - Ideal for EMR, ETL, Log processing,data warehouse
- Cold HDD (sc1) :
    - Suitable for large,sequential IO
    - Baseline - 12 MiB/s/TiB
    - Burst @ 80 MiB/TiB
    - Baseline limit - 250 MiB/s
    - Burst limit - 192 MiB/s



## EC2 - Network And security
- Keypair:
    - 2048-bit SSH-2 RSA keys
    -  five thousand key pairs per region
- Security Group:
    - Virtual firewall 
    - default security group:
        - Allow all outbound
        - Allow all traffic from instances associated with same security group
    - Stateful, if request goes out, the response can come in without creating additional rule
    - Apply rules any time and applied immediately
- Private IP
    - Private IP not reachable over internet
    - Each instance is given internally resolvable dns host name
    - Also can specify secondary private addresses
- Public IP
    - public ip mapped to private ip with NAT
    - instances in default vpc is public ip
    - instances in non default vpc is private ip by default
    - PIP is released on stop and when EIP is associated. In both cases, assigns a new one
- EIP
    - Can be associated with one resource and then assigned to another one
    - When EIP is disassociated, it still remains allocated
    - When instance is running, EIP is not charged
    - Else, stopped and disassociated EIP are charged
- ENI
    - Represents network card attached to an instance
    - Each instance has one primary network interface (eth0) by default which cannot be deleted
    - ENI can be attached and detached to other instance
    - Use Case: Management Network, Load Balancers, High Availability, Dual Homed Instance 
- Placement Groupss:
    - determines how instances are placed on underlying hardware. When you create a placement group, you specify one of the following strategies for the group:
        - Cluster—clusters instances into a low-latency group in a single Availability Zone

        - Spread—spreads instances across underlying hardware



