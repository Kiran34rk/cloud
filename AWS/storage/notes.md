# AWS Storage Services - Detailed Notes

## 1. Amazon S3 (Simple Storage Service)
- **Type:** Object Storage
- **Use Cases:** Backup, archival, data lakes, static website hosting, big data analytics.
- **Features:**
  - Unlimited storage, highly durable (11 9’s durability)
  - Storage classes: Standard, Intelligent-Tiering, Standard-IA, One Zone-IA, Glacier, Glacier Deep Archive
  - Versioning support
  - Lifecycle policies for automated data tiering & deletion
  - Encryption (S3-managed keys, KMS, client-side encryption)
  - Event notifications (trigger Lambda, SNS, SQS)
  - Access control via IAM, Bucket Policies, ACLs
  - Supports multipart upload for large files
  - Strong read-after-write consistency
- **Limitations:** Not suitable for low-latency block storage

## 2. Amazon EBS (Elastic Block Store)
- **Type:** Block Storage
- **Use Cases:** EC2 instance root and data volumes, databases, file systems
- **Features:**
  - Persistent storage attached to EC2 instances
  - Volume types:
    - General Purpose SSD (gp3, gp2)
    - Provisioned IOPS SSD (io2, io1)
    - Throughput Optimized HDD (st1)
    - Cold HDD (sc1)
  - Snapshots for backup (stored in S3)
  - Can be encrypted using AWS KMS
  - Volumes can be resized or modified without downtime (depending on OS support)
  - Supports multi-attach (io2 Block Express only)
- **Limitations:** 
  - Limited to one AZ (cannot span multiple AZs)
  - Maximum size up to 64 TiB per volume

## 3. Amazon EFS (Elastic File System)
- **Type:** Managed Network File System (NFS)
- **Use Cases:** Shared file storage for multiple EC2 instances, containers, and on-premises servers
- **Features:**
  - Scalable elastic storage, automatically grows/shrinks
  - Supports NFSv4 and NFSv4.1 protocols
  - Can be accessed concurrently by multiple instances across AZs
  - Two performance modes: General Purpose, Max I/O
  - Two throughput modes: Bursting, Provisioned
  - Encryption at rest and in transit
  - Integration with AWS IAM for access control
  - Backup support via AWS Backup
- **Limitations:** Higher latency than local disk, suitable for shared workloads

## 4. Amazon FSx
- **Types:**
  - **FSx for Windows File Server**
    - Fully managed Windows native SMB file system
    - Supports Active Directory integration
    - Use for Windows-based applications requiring shared storage
  - **FSx for Lustre**
    - High-performance file system for compute-intensive workloads
    - Integrated with S3 for data import/export
- **Use Cases:** Enterprise file shares, HPC workloads, media processing

## 5. AWS Storage Gateway
- **Type:** Hybrid Cloud Storage
- **Use Cases:** Connect on-premises applications with cloud storage
- **Gateway Types:**
  - File Gateway (NFS/SMB interface, backed by S3)
  - Volume Gateway (block storage interface, backed by EBS snapshots)
  - Tape Gateway (virtual tape library for backup)
- **Features:** Caching, seamless cloud integration, backup, disaster recovery

## 6. Amazon S3 Glacier & Glacier Deep Archive
- **Type:** Archival Object Storage
- **Use Cases:** Long-term archival with infrequent access
- **Features:**
  - Extremely low-cost storage
  - Retrieval options from minutes to hours (Expedited, Standard, Bulk)
  - Vault Lock for compliance and data immutability
  - Encryption at rest by default
- **Limitations:** Higher latency for data retrieval, not suitable for frequent access

## 7. AWS Backup
- **Service for centralized backup management**
- Supports backup of EBS, RDS, DynamoDB, EFS, FSx, Storage Gateway
- Automated backup scheduling, retention management, and lifecycle policies
- Supports cross-region backup and encryption

## Comparison Summary

| Service       | Storage Type       | Access Pattern            | Durability          | Latency        | Multi-AZ Support       | Typical Use Case                          |
|---------------|--------------------|---------------------------|---------------------|----------------|-----------------------|------------------------------------------|
| S3            | Object Storage     | HTTP-based object access   | 99.999999999%       | High           | Global                | Backup, web hosting, big data            |
| EBS           | Block Storage      | Attached to EC2 instances  | 99.999%             | Low            | Single AZ             | Databases, file systems                   |
| EFS           | File Storage (NFS) | Shared file system         | 99.999999999%       | Moderate       | Multi-AZ               | Shared workloads, containers              |
| FSx           | File Storage       | SMB/NFS or Lustre          | High                | Low to Moderate| Multi-AZ (configurable)| Windows file shares, HPC                  |
| Storage Gateway| Hybrid             | On-prem to Cloud Bridge    | Varies              | Depends        | Depends                | Hybrid storage, backup                    |
| Glacier       | Archive Storage    | Archive retrieval          | 99.999999999%       | Hours to Days  | Global                 | Long-term archiving                       |

---

## Important Concepts
- **Durability:** Likelihood of data loss over time (S3 & Glacier offer 11 9’s)
- **Availability:** % uptime of service (S3 Standard ~99.99%)
- **Consistency:** S3 offers strong read-after-write consistency for PUTs and DELETEs
- **Encryption:** Data at rest and in transit encryption options (SSE-S3, SSE-KMS, client-side)
- **Lifecycle Policies:** Automate moving data between storage classes to optimize cost
- **Snapshots:** Point-in-time backups for block storage (EBS)
- **Multi-Attach:** Allows an EBS volume to be attached to multiple instances (only io2 Block Express)
- **Throughput & IOPS:** Key performance metrics, especially for EBS and FSx

---

## Common AWS Storage Use Cases

- **Backup & Disaster Recovery:** Use EBS snapshots, S3, Glacier, and Storage Gateway.
- **Big Data Analytics:** Store raw data in S3, use lifecycle policies to tier to Glacier.
- **Databases:** Use EBS volumes for fast, low-latency block storage.
- **Web Hosting:** Host static websites on S3 with CloudFront CDN.
- **Shared Storage for Containers:** Use EFS or FSx for Windows File Server.
- **Hybrid Cloud:** Storage Gateway to bridge on-premises with cloud storage.

---

# References
- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/index.html)
- [AWS EBS Documentation](https://docs.aws.amazon.com/ebs/index.html)
- [AWS EFS Documentation](https://docs.aws.amazon.com/efs/index.html)
- [AWS FSx Documentation](https://docs.aws.amazon.com/fsx/index.html)
- [AWS Storage Gateway Documentation](https://docs.aws.amazon.com/storagegateway/index.html)
- [AWS Glacier Documentation](https://docs.aws.amazon.com/amazonglacier/latest/dev/what-is-amazon-glacier.html)

