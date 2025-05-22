# AWS Storage Interview Questions & Answers (Detailed)

---

## 1. What are the different types of storage services provided by AWS?

**Answer:**  
AWS offers several storage services optimized for different use cases:

- **Amazon S3:** Object storage, ideal for storing and retrieving any amount of data from anywhere on the web. Used for backups, archives, big data, and static website hosting.
- **Amazon EBS:** Block storage volumes for use with EC2 instances, suitable for databases, file systems, and applications needing persistent storage.
- **Amazon EFS:** Managed Network File System (NFS) for Linux-based workloads that require shared access across multiple instances.
- **Amazon FSx:** Managed file systems including FSx for Windows File Server and FSx for Lustre (high-performance).
- **AWS Storage Gateway:** Hybrid cloud storage connecting on-premises environments with AWS cloud storage.
- **Amazon Glacier & Glacier Deep Archive:** Low-cost archival storage for long-term backups.

---

## 2. Explain Amazon S3 storage classes and when to use each.

**Answer:**

| Storage Class                     | Use Case                                    | Cost & Retrieval                         |
|---------------------------------|---------------------------------------------|-----------------------------------------|
| **S3 Standard**                 | Frequently accessed data                      | Higher cost, instant access              |
| **S3 Intelligent-Tiering**      | Unknown or changing access patterns          | Automatically moves data to lower cost   |
| **S3 Standard-IA (Infrequent Access)** | Less frequently accessed but requires rapid access | Lower cost, retrieval fee applies       |
| **S3 One Zone-IA**              | Infrequently accessed data stored in one AZ | Cheaper, less resilient                  |
| **S3 Glacier Instant Retrieval**| Archival with milliseconds retrieval          | Lowest cost with fast access              |
| **S3 Glacier Flexible Retrieval**| Archive with retrieval in minutes to hours    | Very low cost, slower retrieval          |
| **S3 Glacier Deep Archive**     | Long-term archival (up to 12 hours retrieval) | Lowest cost, slowest retrieval            |

Use lifecycle policies to move objects automatically between classes based on access patterns.

---

## 3. What is the difference between Amazon EBS and Amazon EFS?

**Answer:**

| Feature               | Amazon EBS                         | Amazon EFS                            |
|-----------------------|----------------------------------|-------------------------------------|
| Storage Type          | Block storage                    | Network file storage (NFS)           |
| Access                | Attached to a single EC2 instance | Shared across multiple EC2 instances |
| Use Case              | Boot volumes, databases, transactional workloads | Shared file storage, content management |
| Availability          | AZ-level redundancy              | Multi-AZ redundancy                  |
| Scalability           | Fixed size volumes               | Elastic and scales automatically    |
| Protocol              | Block device                     | NFSv4 protocol                      |

---

## 4. How does Amazon S3 provide high durability?

**Answer:**  
Amazon S3 achieves **11 9's durability (99.999999999%)** by:

- Storing data redundantly across multiple geographically separated Availability Zones (AZs).
- Using checksums to detect data corruption and automatically repairing it from other copies.
- Replicating data synchronously within an AWS Region.
- Versioning allows recovery from accidental deletions or overwrites.

---

## 5. What are Amazon EBS volume types? When to use each?

**Answer:**

- **General Purpose SSD (gp3/gp2):** Balanced price/performance for most workloads (web servers, dev/test).
- **Provisioned IOPS SSD (io1/io2):** High-performance for critical databases requiring low latency and high IOPS.
- **Throughput Optimized HDD (st1):** Low-cost for frequently accessed, throughput-intensive workloads like big data and log processing.
- **Cold HDD (sc1):** Lowest cost, for less frequently accessed data.
- **Magnetic (standard):** Legacy option, rarely used today.

Choose based on required IOPS, throughput, and cost.

---

## 6. How can you secure data stored in S3?

**Answer:**

- **Encryption:**
  - At rest using SSE-S3 (AWS-managed keys), SSE-KMS (customer-managed keys), or SSE-C (customer-provided keys).
  - In transit using HTTPS/TLS.
- **Access Control:**
  - IAM policies and roles.
  - Bucket policies with resource-based permissions.
  - Access Control Lists (ACLs).
- **Logging & Monitoring:**
  - Enable S3 server access logging.
  - Use AWS CloudTrail for API activity logging.
- **Network Restrictions:**
  - Use VPC endpoints for private connectivity.
  - Use bucket policies to restrict access by IP or VPC.

---

## 7. What is the difference between an S3 bucket policy and IAM policy?

**Answer:**

- **IAM Policy:** Attached to users, groups, or roles; controls what AWS resources the identity can access.
- **Bucket Policy:** Resource-based policy attached directly to an S3 bucket; controls who can access the bucket and what actions they can perform on it, regardless of the IAM user’s permissions.

Bucket policies are useful for cross-account access or public access settings.

---

## 8. What is a lifecycle policy in S3 and why is it useful?

**Answer:**

An S3 lifecycle policy is a rule that automates the transition of objects between storage classes or expiration of objects after a set time.

**Benefits:**

- Reduces storage costs by moving older data to cheaper storage classes (e.g., Glacier).
- Automatically deletes objects to manage storage footprint.
- Helps implement data retention policies and compliance.

---

## 9. Can you attach one EBS volume to multiple EC2 instances simultaneously?

**Answer:**  
By default, **no**, EBS volumes can only be attached to a single EC2 instance at a time.  
However, **io2 volumes with Multi-Attach** enabled support attaching a single volume to multiple instances within the same AZ for clustered applications.

---

## 10. Explain Amazon EFS performance modes.

**Answer:**

- **General Purpose:** Default mode, suitable for latency-sensitive use cases like web serving, content management.
- **Max I/O:** Higher throughput and IOPS, ideal for highly parallelized workloads such as big data analytics or media processing, but with slightly higher latency.

---

## 11. How does AWS Storage Gateway help in hybrid cloud storage?

**Answer:**

AWS Storage Gateway allows on-premises applications to seamlessly use AWS cloud storage via:

- **File Gateway:** NFS/SMB interface to S3 for file storage and backup.
- **Volume Gateway:** iSCSI interface for block storage backed by S3.
- **Tape Gateway:** Virtual tape library for backup software integration with Glacier.

This bridges on-premises infrastructure with scalable AWS cloud storage without modifying applications.

---

## 12. What is the difference between S3 Glacier and S3 Glacier Deep Archive?

**Answer:**

| Feature               | S3 Glacier                      | S3 Glacier Deep Archive          |
|-----------------------|--------------------------------|---------------------------------|
| Retrieval Time        | Minutes to hours               | Up to 12 hours                  |
| Cost per GB/month     | Low                           | Lowest                         |
| Use Case              | Archival, infrequent access   | Long-term archival, rarely accessed data |

---

## 13. How does AWS Backup integrate with storage services?

**Answer:**

AWS Backup provides centralized backup management for:

- EBS volumes
- RDS databases
- DynamoDB tables
- EFS file systems
- Storage Gateway volumes

It automates backup scheduling, retention policies, cross-region backups, and compliance auditing.

---

## 14. What is strong consistency in Amazon S3 and why is it important?

**Answer:**

Strong read-after-write consistency means that after a write or update, any subsequent read immediately reflects the latest data.  
It is important for applications that need the latest data to be visible immediately after upload, such as web apps or transactional data.

AWS S3 supports this model for all PUTS of new objects and PUTS/DELETES of existing objects.

---

## 15. How do you optimize costs in AWS storage services?

**Answer:**

- Use appropriate storage classes (e.g., move cold data to Glacier).
- Implement lifecycle policies for automatic data tiering and expiration.
- Delete unused snapshots and volumes.
- Use S3 Intelligent-Tiering for unpredictable access patterns.
- Right-size EBS volumes and select proper volume types.
- Use compression and deduplication where possible.

---

# Additional Notes

- Always consider **data durability**, **availability**, **performance**, and **cost** when designing storage solutions.
- Security best practices include encryption, strict IAM policies, network controls, and monitoring.
- AWS regularly updates features and pricing—stay current via AWS announcements and documentation.

---

# References

- [AWS Storage Services Overview](https://aws.amazon.com/products/storage/)
- [Amazon S3 Documentation](https://docs.aws.amazon.com/s3/index.html)
- [Amazon EBS Documentation](https://docs.aws.amazon.com/ebs/index.html)
- [Amazon EFS Documentation](https://docs.aws.amazon.com/efs/index.html)
- [AWS Storage Gateway Documentation](https://docs.aws.amazon.com/storagegateway/index.html)
- [AWS Backup Documentation](https://docs.aws.amazon.com/aws-backup/index.html)

