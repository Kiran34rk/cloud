# 🧠 Amazon EBS (Elastic Block Store) – Detailed Notes

---

## 🔹 1. What is Amazon EBS?

- Amazon EBS is a **block storage** service for **EC2 instances**.
- It provides **durable, high-performance** volumes that persist independently of EC2.
- Common use cases:
  - Databases (MySQL, Oracle, MongoDB)
  - File systems
  - Boot volumes
  - Enterprise applications

---

## 🔹 2. EBS Volume Types

| Type         | Use Case                                 | Performance                         | Notes                              |
|--------------|-------------------------------------------|--------------------------------------|-------------------------------------|
| gp3          | General purpose SSD                       | Up to 16,000 IOPS, 1,000 MB/s        | Recommended for most workloads      |
| gp2          | General purpose SSD                       | 3 IOPS per GB (up to 16,000 IOPS)    | Performance tied to size            |
| io2 / io2 Block Express | High-performance SSD            | Up to 256,000 IOPS                   | For IOPS-intensive workloads         |
| st1          | Throughput optimized HDD                  | Max 500 MB/s                         | Big data, logs                      |
| sc1          | Cold HDD                                  | Max 250 MB/s                         | Infrequent access, low-cost         |
| Magnetic     | Deprecated                                | Low throughput                       | Legacy only                         |

---

## 🔹 3. Key Features

- **Durability**: 99.999% availability; auto-replicated within AZ.
- **Scalability**: Resize without downtime.
- **Snapshots**: Point-in-time backups to S3.
- **Encryption**: AWS KMS-managed encryption at rest and transit.
- **Lifecycle Management**: Automate snapshots with DLM.
- **Multi-Attach**: io1/io2 volumes support multi-attach to EC2.

---

## 🔹 4. Volume Lifecycle

1. **Create Volume**
2. **Attach to EC2**
3. **Mount & Format**
4. **Use**
5. **Snapshot**
6. **Delete**

---

## 🔹 5. Performance Concepts

- **IOPS**: Input/output operations per second.
- **Throughput**: Data transfer rate (MB/s).
- **Bursting**:
  - gp2 and st1 use burst credits.
  - gp3 allows provisioned IOPS and throughput.

---

## 🔹 6. Snapshots

- **Stored in S3**
- **Incremental**: Only changes since last snapshot.
- **Use cases**:
  - Backups
  - Volume restoration
  - Cross-region copy

### 🔸 Automated Snapshots
- Via **AWS Backup** or **Data Lifecycle Manager (DLM)**.

---

## 🔹 7. Encryption

- Uses **AWS KMS** (default or custom keys).
- Encrypt:
  - At creation
  - Snapshots
  - New volumes from encrypted snapshot
- Cannot encrypt an existing unencrypted volume directly.

---

## 🔹 8. Resizing and Modifying Volumes

- Use AWS Console or CLI (`modify-volume`).
- Post-resize: Expand file system using:
  - `resize2fs` for ext4
  - `xfs_growfs` for xfs

---

## 🔹 9. EBS vs Instance Store

| Feature              | EBS                             | Instance Store                    |
|----------------------|----------------------------------|----------------------------------|
| Durability           | Persistent                       | Ephemeral                        |
| Data Persistence     | Yes                              | No                               |
| Backup               | Snapshot support                 | No backup                        |
| Size Flexibility     | Configurable                     | Fixed size                       |
| Use Cases            | Databases, logs, apps            | Caching, temporary data          |

---

## 🔹 10. Pricing Model

You are charged for:
- **Provisioned storage (GB/month)**
- **Provisioned IOPS** (for gp3/io2)
- **Snapshot storage**
- **Cross-region snapshot transfers**

---

## 🔹 11. Monitoring (via CloudWatch)

Key metrics:
- `VolumeReadOps`
- `VolumeWriteOps`
- `VolumeQueueLength`
- `BurstBalance`
- `VolumeThroughputPercentage`

---

## 🔹 12. AWS CLI Commands

```bash
# Create EBS volume
aws ec2 create-volume \
  --availability-zone us-east-1a \
  --size 20 \
  --volume-type gp3

# Attach to EC2 instance
aws ec2 attach-volume \
  --volume-id vol-xxxxxxxx \
  --instance-id i-xxxxxxxx \
  --device /dev/xvdf

# Create snapshot
aws ec2 create-snapshot \
  --volume-id vol-xxxxxxxx \
  --description "Backup snapshot"

# Modify volume
aws ec2 modify-volume \
  --volume-id vol-xxxxxxxx \
  --size 100
