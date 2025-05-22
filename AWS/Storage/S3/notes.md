# Notes
📌 Overview
Amazon S3 is an object storage service that offers scalability, data availability, security, and performance.

Stores objects in buckets.

Each object consists of data, metadata, and a unique identifier (key).

🪣 Buckets
✅ Key Concepts:
Buckets are global and unique across all AWS accounts.

Bucket names follow DNS naming conventions.

One account can have up to 100 buckets by default.

No nesting of buckets (flat structure).

📦 Bucket Operations:
Create / Delete

Set Bucket Policies, ACLs

Enable Versioning, Logging, Replication

📁 Objects
✅ Object Details:
Each object has:

Key: Unique identifier (like a filename)

Value: Data (max 5TB)

Metadata: System-defined and user-defined

Version ID (if versioning enabled)

Multipart Upload for files >5GB

🔒 Security & Access Control
1. IAM Policies:
Define permissions at the user or role level.

2. Bucket Policies:
JSON-based policies to allow/deny access to buckets and objects.

3. ACLs (Access Control Lists):
Legacy feature, allow permission settings at object level.

4. S3 Block Public Access:
Blocks public access at account or bucket level.

5. Encryption:
At rest:

SSE-S3: Managed keys by AWS

SSE-KMS: Customer-managed keys

SSE-C: Customer-provided keys

In transit:

Uses SSL/TLS

🗂️ Storage Classes
Class    Use Case    Availability    Durability    Retrieval Time    Cost
S3 Standard    Frequently accessed    99.99%    99.999999999%    Immediate    High
S3 Intelligent-Tiering    Unknown patterns    99.9–99.99%    99.999999999%    Immediate    Auto-optimizes cost
S3 Standard-IA    Infrequent access    99.9%    99.999999999%    Immediate    Lower cost
S3 One Zone-IA    Infrequent, non-critical    99.5%    99.999999999%    Immediate    Lower cost
S3 Glacier    Archival    NA    99.999999999%    Minutes–Hours    Very low
S3 Glacier Deep Archive    Long-term archival    NA    99.999999999%    12 hours    Cheapest

🌀 Versioning
Keeps multiple versions of an object.

Helps in data recovery and protection against accidental deletes.

Must be enabled at bucket creation (irreversible setting).

🔁 Cross-Region Replication (CRR)
Automatically replicates objects across AWS regions.

Requires versioning enabled.

Useful for disaster recovery and compliance.

📜 Lifecycle Policies
Automates transition between storage classes or expiration.

Example: Move to Glacier after 30 days, delete after 365 days.

🔍 Event Notifications
Trigger SNS, SQS, or Lambda on events like:

Object Created

Object Removed

Use cases: Auto processing, auditing, alerting

📈 Logging & Monitoring
1. S3 Access Logs
Records request details.

Stored in another bucket.

2. CloudTrail
Captures API calls for auditing.

3. CloudWatch
Monitor metrics like request count, latency, errors.

🔗 Presigned URLs
Temporarily grant access to objects with an expiry time.

Useful for secure downloads/uploads without making objects public.

bash
Copy
Edit
aws s3 presign s3://my-bucket/my-file.txt --expires-in 3600
🧰 Common CLI Commands
bash
Copy
Edit
# Upload file
aws s3 cp file.txt s3://my-bucket/

# Download file
aws s3 cp s3://my-bucket/file.txt .

# Sync folders
aws s3 sync ./local-folder s3://my-bucket/ --delete

# List objects
aws s3 ls s3://my-bucket/

# Remove object
aws s3 rm s3://my-bucket/file.txt
🧠 Interview Tips
Know difference between storage classes.

Understand versioning, replication, encryption well.

Be prepared to explain security: IAM vs Bucket Policy vs ACLs.

Be ready with S3 + Lambda use case (e.g., image processing).

Mention Presigned URLs for secure file sharing.

Explain Lifecycle Policies to manage costs.

🧱 S3 Data Consistency Model
Strong consistency for all PUT, DELETE, and LIST operations.

Ensures immediate read-after-write consistency.

Previously, S3 was eventually consistent, but now fully consistent for all operations globally.

📦 S3 Object Lock (WORM)
🔐 What it does:
Prevents objects from being deleted or overwritten for a defined retention period.

🛠️ Use Cases:
Financial data

Compliance requirements (e.g., SEC Rule 17a-4(f), HIPAA)

🔧 Modes:
Governance Mode: Users with special permissions can delete/modify.

Compliance Mode: Nobody can modify/delete until the lock expires.

🚨 S3 Access Points
Create unique entry points to a bucket for specific applications or teams.

Simplifies managing access for shared datasets.

Access policies can be applied at the access point level.

bash
Copy
Edit
aws s3control create-access-point \
  --name my-app-access \
  --bucket my-data-bucket \
  --region us-west-2
🧪 S3 Select
🎯 Purpose:
Retrieve subset of data (columns/rows) from CSV, JSON, or Parquet stored in S3 using SQL-like queries.

✅ Benefits:
Reduces bandwidth and cost.

Speeds up data retrieval by processing on AWS side.

🧬 Example:
sql
Copy
Edit
SELECT s.* FROM S3Object s WHERE s.age > 30
Use with AWS SDK or AWS CLI.

🚀 S3 Performance Optimization
Prefix Parallelization:

S3 scales based on object key name prefixes.

Avoid performance bottlenecks by spreading read/write across prefixes.

Multipart Upload:

Speeds up upload of large files.

Enables resuming interrupted uploads.

Transfer Acceleration:

Speeds up uploads over long distances using CloudFront edge locations.

Use s3-accelerate.amazonaws.com endpoint.

🛡️ Data Protection Strategies
Feature    Description
Versioning    Recover deleted or overwritten files
Cross-Region Replication (CRR)    Disaster recovery, global access
Object Lock    WORM for compliance
S3 Backup to Glacier    Long-term archival
MFA Delete    Prevent accidental deletes (via MFA)

🌍 Cross-Origin Resource Sharing (CORS)
Controls how S3 allows browser-based access from other domains.

Example: Allow example.com to access s3.amazonaws.com/my-bucket.

xml
Copy
Edit
<CORSRule>
  <AllowedOrigin>https://example.com</AllowedOrigin>
  <AllowedMethod>GET</AllowedMethod>
  <AllowedHeader>*</AllowedHeader>
</CORSRule>
🌐 Static Website Hosting
S3 can host static websites (HTML, CSS, JS only).

Enable via bucket properties.

URL format:
http://<bucket-name>.s3-website-<region>.amazonaws.com

Important:
Must disable Block Public Access

Use bucket policy to allow s3:GetObject

🧮 S3 Pricing Breakdown
Component    Charged for?
Storage    ✅ Yes
PUT/COPY/POST/LIST requests    ✅ Yes
GET/SELECT requests    ✅ Yes (but cheaper)
Data transfer OUT    ✅ Yes
Data transfer IN    ❌ Free
Lifecycle transitions    ✅ Yes
Replication (CRR)    ✅ Yes

🧰 Real-World Use Cases
Data Lake:

Store raw data from multiple sources.

Use Athena, Redshift Spectrum, or EMR for analytics.

Backup & Restore:

Store EBS snapshot copies, RDS backups.

Machine Learning:

Store training datasets; integrate with SageMaker.

Web Content Hosting:

Host static sites, media files, or image galleries.

IoT Data Storage:

Devices upload data directly to S3 using presigned URLs.


