# AWS EBS Interview Questions and Answers

## Basic Concepts

### Q1: What is AWS EBS?
**A:**  
AWS Elastic Block Store (EBS) is a persistent block-level storage service designed for use with Amazon EC2 instances. It provides highly available, reliable, and scalable storage volumes that can be attached to running EC2 instances in the same Availability Zone.

### Q2: How does EBS differ from instance store?
**A:**  
| Feature        | EBS                          | Instance Store               |
|---------------|-----------------------------|-----------------------------|
| Persistence   | Persistent (survives instance termination) | Ephemeral (lost on instance stop/termination) |
| Performance   | Network-attached (slightly slower) | Physically attached (faster) |
| Availability | Replicated within AZ          | Not replicated              |
| Resizing     | Can be resized dynamically    | Fixed size                  |
| Cost         | Billed separately            | Included in instance cost   |

## Volume Types

### Q3: Explain the different EBS volume types
**A:**  
1. **SSD-backed**:
   - **gp3**: General purpose, baseline 3,000 IOPS, scalable to 16,000 IOPS
   - **io2/io1**: Provisioned IOPS (PIOPS), up to 256,000 IOPS (io2)
   
2. **HDD-backed**:
   - **st1**: Throughput optimized (500 MB/s max)
   - **sc1**: Cold HDD (lowest cost, 250 MB/s max)

### Q4: When would you choose io2 over gp3?
**A:**  
Choose io2 when you need:
- Consistent high performance (>16,000 IOPS)
- Multi-attach capability
- Mission-critical workloads requiring 99.999% durability
- Predictable performance for databases like Oracle or SAP HANA

## Snapshots and Backup

### Q5: How do EBS snapshots work?
**A:**  
- Point-in-time copies of EBS volumes
- Stored incrementally (only changed blocks since last snapshot)
- Stored in S3 (but not visible in S3 console)
- Can create new volumes or AMIs from snapshots
- Can be shared across accounts/regions

### Q6: What's the difference between EBS snapshots and AMIs?
**A:**  
- **EBS Snapshot**: Backup of a single volume
- **AMI (Amazon Machine Image)**: 
  - Includes snapshots of all attached EBS volumes
  - Contains instance metadata (kernel, RAM disk, permissions)
  - Used to launch new EC2 instances

## Performance

### Q7: How can you improve EBS performance?
**A:**  
- Use EBS-optimized EC2 instances
- For gp3: Increase IOPS/throughput beyond baseline
- For io2: Provision sufficient IOPS for workload
- Use RAID 0 for striping (combine multiple volumes)
- Ensure instances and volumes are in the same AZ
- For Linux: Use `fio` for benchmarking and optimize filesystem settings

### Q8: Explain EBS burst performance
**A:**  
- **gp2 volumes** use a burst bucket mechanism:
  - Earn credits at baseline rate (3 IOPS per GB)
  - Spend credits during bursts (up to 3,000 IOPS)
  - Example: 100GB volume has 300 IOPS baseline, can burst to 3,000 IOPS
- **gp3 volumes** have fixed baseline performance (3,000 IOPS) without bursting

## Advanced Features

### Q9: What is EBS Multi-Attach?
**A:**  
- Allows a single io1/io2 volume to be attached to multiple Nitro-based EC2 instances
- All instances must be in the same AZ
- Use cases: Clustered applications (e.g., Oracle RAC, Windows Failover Cluster)
- Requires clustered filesystem to handle concurrent writes

### Q10: How does EBS encryption work?
**A:**  
- Uses AES-256 encryption
- Keys are managed through AWS KMS
- Minimal performance impact (<10%)
- All snapshots and volumes created from encrypted volumes are automatically encrypted
- Data is encrypted at rest and in transit between EC2 and EBS

## Troubleshooting

### Q11: Your EBS volume shows high latency. How would you diagnose?
**A:**  
1. Check CloudWatch metrics:
   - `VolumeQueueLength` (should be < 1 for optimal performance)
   - `VolumeTotalReadTime`/`VolumeTotalWriteTime`
2. Verify instance is EBS-optimized
3. Check if volume is hitting IOPS/throughput limits
4. For gp2: Check burst balance (`BurstBalance` metric)
5. For Linux: Use `iostat -x` to check await time

### Q12: How would you migrate an EBS volume to another AZ?
**A:**  
1. Take a snapshot of the volume
2. Copy the snapshot to the target region (if cross-region)
3. Create a new volume from the snapshot in the target AZ
4. Attach the new volume to an instance in the target AZ

## Best Practices

### Q13: What are key EBS best practices?
**A:**  
- **Right-size volumes**: Choose appropriate type/size for workload
- **Monitor metrics**: IOPS, throughput, burst balance
- **Use snapshots**: For backups and migration
- **RAID 0**: For high-performance needs (combine multiple volumes)
- **Stagger snapshots**: For applications with many volumes
- **Pre-warm volumes**: For restored volumes (read all blocks before use)

### Q14: When would you use st1 vs sc1 volumes?
**A:**  
- **st1 (Throughput Optimized)**:
  - Frequently accessed, throughput-intensive workloads
  - Big data, data warehouses, log processing
  - Not suitable for boot volumes
  
- **sc1 (Cold HDD)**:
  - Less frequently accessed data
  - Lowest cost archival storage
  - Can tolerate higher latency
  - Not suitable for boot volumes

## Pricing

### Q15: What factors affect EBS pricing?
**A:**  
- **Storage amount**: GB provisioned per month
- **Volume type**: gp3 is cheaper than io2 for same size
- **IOPS**: Additional cost for provisioned IOPS (io1/io2 and extra gp3 IOPS)
- **Throughput**: Extra cost for gp3 throughput beyond 125 MB/s
- **Snapshots**: Based on stored data in S3
- **Data transfer**: Outbound and cross-region transfers
