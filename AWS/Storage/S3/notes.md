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
Component	Charged for?
Storage	✅ Yes
PUT/COPY/POST/LIST requests	✅ Yes
GET/SELECT requests	✅ Yes (but cheaper)
Data transfer OUT	✅ Yes
Data transfer IN	❌ Free
Lifecycle transitions	✅ Yes
Replication (CRR)	✅ Yes

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
