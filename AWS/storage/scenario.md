# AWS Storage Scenario-Based Interview Questions & Answers

---

## 1. Scenario: You need to store large amounts of user-generated images and videos with varying access patterns. Some files are accessed frequently right after upload, but most become infrequently accessed over time. How would you design your AWS storage solution?

**Answer:**  
- Use **Amazon S3** as the primary storage for its scalability and durability.
- Set the default storage class to **S3 Standard** for immediate frequent access after upload.
- Implement **Lifecycle policies** to automatically transition objects to **S3 Standard-IA** or **S3 Glacier** after a set period (e.g., 30 days).
- Enable **versioning** to protect against accidental deletion or overwrites.
- Use **S3 Intelligent-Tiering** if access patterns are unpredictable, which automatically moves objects between frequent and infrequent access tiers to optimize cost.
- Secure the bucket with proper **IAM policies**, **bucket policies**, and encryption.

---

## 2. Scenario: Your company runs a transactional database on EC2 that requires low latency and high IOPS. The database is critical and needs to be backed up regularly without affecting performance. What AWS storage service and features do you recommend?

**Answer:**  
- Use **Amazon EBS Provisioned IOPS SSD (io2/io2 Block Express)** volumes to provide high and consistent IOPS and low latency.
- Attach the EBS volume directly to the EC2 instance for block-level storage.
- Enable **EBS Multi-Attach** if the database supports clustered or shared storage to allow multiple EC2 instances to access the volume concurrently (optional).
- Use **EBS snapshots** for backups:
  - Schedule regular snapshots using AWS Backup or custom automation.
  - Snapshots are incremental and do not impact performance significantly.
- Encrypt EBS volumes using **AWS KMS** for data security.
- Consider using **Amazon RDS** if managed database service is an option.

---

## 3. Scenario: You need a shared file system accessible concurrently by hundreds of EC2 Linux instances running a big data analytics workload. The workload is highly parallel and needs high throughput. What storage solution would you choose?

**Answer:**  
- Choose **Amazon EFS** for shared file storage accessible concurrently by multiple EC2 instances.
- Use the **Max I/O performance mode** in EFS to scale to higher throughput and IOPS for highly parallel workloads.
- Use **Provisioned Throughput mode** if predictable throughput above the default burst is required.
- Enable encryption at rest and in transit for security.
- Leverage lifecycle policies to move older files to EFS Infrequent Access storage class to reduce costs.
- Mount the EFS file system on all EC2 instances using the NFS protocol.

---

## 4. Scenario: Your company wants to move an on-premises backup system to AWS but maintain seamless integration with existing backup software and workflows. What service would you use and how?

**Answer:**  
- Use **AWS Storage Gateway**, specifically the **Tape Gateway** if the existing backup system uses tape backups.
- Tape Gateway presents a virtual tape library interface compatible with common backup software.
- Backups are stored in Amazon S3 and archived to Amazon Glacier or Glacier Deep Archive for long-term retention.
- This approach allows minimal changes to on-premises backup workflows while leveraging AWS durability and scalability.
- Alternatively, **File Gateway** can be used for file-based backups with NFS/SMB access.
- Ensure proper network setup, including VPN or Direct Connect, for secure hybrid integration.

---

## 5. Scenario: Your organization needs a highly available file storage solution for a Windows-based application requiring SMB protocol and Active Directory integration. What AWS service should you use?

**Answer:**  
- Use **Amazon FSx for Windows File Server** which provides a fully managed native Windows file system.
- Supports SMB protocol and integrates with Microsoft Active Directory for authentication and access control.
- Automatically replicates data within and across Availability Zones for high availability.
- Offers encryption at rest and in transit.
- Scales automatically and supports typical Windows file system features like ACLs, DFS namespaces, and shadow copies.

---

## 6. Scenario: You have a compliance requirement to retain data backups for 7 years but want to minimize storage costs. Retrieval of these backups is expected to be rare but may occasionally be needed within hours. How would you implement this in AWS?

**Answer:**  
- Use **Amazon S3 Glacier Flexible Retrieval** (formerly Glacier) for archival storage.
- Set lifecycle policies on S3 buckets or backups to automatically transition data to Glacier after a short retention period.
- Glacier provides very low-cost storage with retrieval options ranging from minutes to hours.
- For backups requiring occasional fast retrieval, **Glacier Instant Retrieval** class can be considered.
- Implement **AWS Backup** to centrally manage backup retention policies.
- Encrypt backups and enable audit logging for compliance.
- Use lifecycle policies or backup vault lock features to enforce retention and prevent early deletion.

---

## 7. Scenario: Your application requires strong read-after-write consistency for critical data but also needs to serve a global audience with low latency reads. How would you architect your storage?

**Answer:**  
- Use **Amazon S3** as it provides strong read-after-write consistency within a region.
- To serve a global audience with low latency, use **Amazon CloudFront CDN** in front of S3 buckets.
- For global write consistency, consider using **Amazon S3 Cross-Region Replication (CRR)** for disaster recovery and geo-redundancy (note CRR replicates asynchronously).
- Alternatively, architect application logic to write data in the closest region and read locally.
- Use **AWS Global Accelerator** or Route 53 latency-based routing for efficient user request routing.
- Maintain proper caching and invalidation strategies on CloudFront for consistency.

---

## 8. Scenario: You have an application running on Kubernetes (EKS) that requires persistent storage with dynamic provisioning and snapshots for backup. What AWS storage solution would you use?

**Answer:**  
- Use **Amazon EBS** as the Persistent Volume (PV) backend because EBS supports dynamic provisioning through the Kubernetes CSI driver.
- Create **StorageClasses** in Kubernetes that specify EBS volume types (gp3, io2) for different workloads.
- Use **Kubernetes VolumeSnapshot** feature with EBS CSI driver to create point-in-time backups.
- For shared access workloads, consider **Amazon EFS CSI driver** to mount EFS volumes to pods.
- Manage backups using Kubernetes-native tools or integrate with AWS Backup.
- Ensure proper IAM roles and permissions for Kubernetes nodes to interact with EBS and snapshots.

---

## 9. Scenario: Your company wants to optimize storage costs by archiving old but critical data while ensuring occasional retrieval is possible within minutes. Data is currently stored on EBS volumes. What is your approach?

**Answer:**  
- Take **EBS snapshots** of the volumes storing old data.
- Use **EBS Snapshot Lifecycle policies** to automate snapshot creation and deletion.
- Export snapshots to **Amazon S3 Glacier Instant Retrieval** or **Glacier Flexible Retrieval** using **EBS snapshot archive** feature.
- Delete old EBS volumes after ensuring snapshots are properly archived.
- When needed, restore snapshots from Glacier back to EBS volumes.
- This reduces cost while maintaining backup and restore capability within acceptable retrieval times.

---

## 10. Scenario: A multi-tenant SaaS application stores customer data in S3. Each tenant's data must be isolated and access controlled strictly. How do you design access control?

**Answer:**  
- Use **S3 bucket policies** or **IAM policies** with **resource-level permissions** scoped by tenant prefixes (e.g., s3://bucket/tenantA/, s3://bucket/tenantB/).
- Use **IAM roles** with least privilege principle for each tenant’s application or user access.
- Implement **AWS Cognito** or other identity federation to assign temporary credentials scoped to tenant-specific access.
- Enable **S3 Access Points** per tenant to simplify policy management.
- Use encryption keys managed via **AWS KMS** with key policies granting tenant-specific access.
- Enable logging and monitoring with **CloudTrail** and **S3 Access Logs** for audit.

---

# Summary

These scenario-based questions test your ability to choose and architect AWS storage services based on real-world requirements, balancing cost, performance, durability, security, and compliance.

---

# References

- [Amazon S3 Storage Classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)  
- [Amazon EBS Volume Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html)  
- [Amazon EFS Performance Modes](https://docs.aws.amazon.com/efs/latest/ug/performance.html)  
- [AWS Storage Gateway](https://aws.amazon.com/storagegateway/)  
- [AWS Backup](https://aws.amazon.com/backup/)  
- [Kubernetes CSI Driver for AWS](https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html)  

