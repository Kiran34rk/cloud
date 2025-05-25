# AWS EBS (Elastic Block Store) - Detailed Notes

## Overview
- **Purpose**: Provides persistent block storage volumes for use with EC2 instances
- **Characteristics**:
  - Network-attached storage (not instance storage)
  - Persists independently from instance life
  - Replicated within a single Availability Zone (AZ)
  - Can be attached to only one EC2 instance at a time (except multi-attach)

## Volume Types

### SSD-backed Volumes
1. **gp3 (General Purpose SSD)**
   - Baseline performance: 3,000 IOPS, 125 MB/s throughput
   - Scalable up to 16,000 IOPS and 1,000 MB/s (independent of volume size)
   - Cost-effective for most workloads
   - Default EBS volume type

2. **gp2 (Previous Generation)**
   - Performance scales with volume size (3 IOPS per GB)
   - Burst capability up to 3,000 IOPS
   - Max 16,000 IOPS and 250 MB/s throughput

3. **io1/io2 (Provisioned IOPS SSD)**
   - High-performance for mission-critical apps
   - Supports up to 64,000 IOPS (io1) or 256,000 IOPS (io2)
   - io2 has better durability (99.999% vs 99.8-99.9%)
   - Supports Multi-Attach (up to 16 instances)

### HDD-backed Volumes
1. **st1 (Throughput Optimized HDD)**
   - Low-cost HDD for frequently accessed, throughput-intensive workloads
   - Max throughput 500 MB/s
   - Not suitable for boot volumes

2. **sc1 (Cold HDD)**
   - Lowest cost option for less frequently accessed data
   - Max throughput 250 MB/s
   - Not suitable for boot volumes

## Key Features

### Snapshots
- **Point-in-time copies** of EBS volumes
- Stored in S3 (but not visible in S3 console)
- **Incremental** - only stores changed blocks since last snapshot
- Can create new volumes or AMIs from snapshots
- Can be shared across AWS accounts
- Can be copied across regions

### Encryption
- **AES-256 encryption** at rest
- Encryption occurs on servers hosting EC2 instances
- Minimal performance impact
- Encrypted volumes can only be attached to supported instance types

### Multi-Attach
- Available only for **io1/io2** volumes
- Single volume can be attached to **multiple Nitro-based instances**
- All instances must be in the same AZ
- Use cases: clustered applications (e.g., Oracle RAC)

### Lifecycle Manager
- Automates creation, retention, and deletion of EBS snapshots
- Can create policies based on tags
- Helps meet compliance requirements

## Performance Considerations

### IOPS and Throughput
- **IOPS**: Input/output operations per second
- **Throughput**: Data transfer rate (MB/s)
- SSD volumes measured in IOPS
- HDD volumes measured in MB/s

### Burst Performance
- gp2 volumes use burst buckets
- Earn credits at baseline rate (3 IOPS/GB)
- Spend credits during bursts (up to 3,000 IOPS)

### Best Practices
- Use EBS-optimized instances for consistent performance
- For high performance: Use Provisioned IOPS (io1/io2)
- For throughput-heavy workloads: Use st1
- Monitor using CloudWatch metrics:
  - `VolumeReadBytes`, `VolumeWriteBytes`
  - `VolumeReadOps`, `VolumeWriteOps`
  - `VolumeTotalReadTime`, `VolumeTotalWriteTime`

## Pricing Factors
- **Volume type** (gp3, io2, st1, etc.)
- **Storage amount** (GB provisioned per month)
- **IOPS** (for io1/io2 and additional gp3 IOPS)
- **Throughput** (for additional gp3 throughput)
- **Snapshots** (based on stored data)
- **Data transfer** (outbound and cross-region)

## Limits
- **Volume size**: 1 GiB to 16 TiB
- **Maximum IOPS**:
  - gp3: 16,000
  - io1: 64,000
  - io2: 256,000
- **Attachments per instance**: Up to 40 volumes (instance type dependent)

## Use Cases
- **Boot volumes** for EC2 instances
- **Database storage** (RDS uses EBS under the hood)
- **Enterprise applications** requiring persistent storage
- **Big data analytics** workloads
- **Container storage** for stateful containers
