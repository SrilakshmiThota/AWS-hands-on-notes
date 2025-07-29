
Amazon Simple Storage Service
AWS S3 - L1 and L2 Troubleshooting

---

## L1 Support Responsibilities for AWS S3

### 1. ❓ Basic Access Issues
- **Issue**: User unable to access S3 bucket or file.
- **Checks**:
  - Verify IAM permissions (e.g., `s3:GetObject`, `s3:ListBucket`)
  - Ensure user is logged in to correct AWS account.
  - Check for bucket policy blocking access.
- **Resolution**: Grant required permissions or correct user role.

### 2. 📁 Object Not Found
- **Issue**: Object missing from the bucket.
- **Checks**:
  - Confirm correct object key name and path.
  - Use `aws s3 ls s3://bucket-name/path/` to verify presence.
- **Resolution**: Guide user to correct path or recover/upload the object.

### 3. 🔒 Permission Denied on Upload/Download
- **Issue**: Access Denied error during upload/download.
- **Checks**:
  - IAM role or bucket policy missing permissions like `s3:PutObject` or `s3:GetObject`.
  - Check bucket ACLs and public access block settings.
- **Resolution**: Modify policy or ACL as per requirement.

### 4. ⏳ Slow Uploads/Downloads
- **Issue**: Users reporting slow transfer speeds.
- **Checks**:
  - Network bandwidth issues.
  - Large object sizes (suggest multipart upload for >100MB).
- **Resolution**: Recommend AWS CLI or S3 Transfer Acceleration.

### 5. 📦 Storage Class Confusion
- **Issue**: Object storage class mismatch or data not accessible.
- **Checks**:
  - Check object metadata for storage class (Standard, IA, Glacier).
  - Check lifecycle rules.
- **Resolution**: Educate user about delays in retrieval (e.g., Glacier).

---

## L2 Support Responsibilities for AWS S3

### 1. 🔁 S3 Versioning & Data Recovery
- **Issue**: File was overwritten or deleted.
- **Checks**:
  - Confirm if versioning is enabled.
  - Use `aws s3api list-object-versions` to retrieve old versions.
- **Resolution**: Restore previous version or guide user to enable versioning.

### 2. 🔄 Cross-Region Replication Issues
- **Issue**: Replication not working between source and destination buckets.
- **Checks**:
  - Confirm replication rule is enabled and configured properly.
  - IAM roles for replication must have necessary trust and S3 permissions.
- **Resolution**: Reconfigure or redeploy replication rules.

### 3. 🔍 S3 Access Logs Not Available
- **Issue**: Logs not appearing in destination bucket.
- **Checks**:
  - Confirm logging is enabled on source bucket.
  - Verify destination bucket permissions (`s3:PutObject`).
- **Resolution**: Fix log bucket policy and permissions.

### 4. 🛡️ S3 Bucket Policy Misconfiguration
- **Issue**: Unexpected access (too open or restricted).
- **Checks**:
  - Inspect and review bucket policy with tools like IAM Policy Simulator.
- **Resolution**: Update policy for least privilege or restrict public access.

### 5. 🕵️‍♀️ Object Lock & Legal Hold Troubleshooting
- **Issue**: Objects cannot be deleted or modified.
- **Checks**:
  - Check if object lock is enabled.
  - Verify mode (Compliance vs Governance).
- **Resolution**: Explain lock restrictions or request admin override (if allowed).

### 6. 🧾 Lifecycle Policies Not Working
- **Issue**: Data not transitioning to another storage class.
- **Checks**:
  - Review lifecycle rules using `aws s3api get-bucket-lifecycle-configuration`.
- **Resolution**: Correct rule configuration and monitor delays (up to 48 hours).

### 7. 🧰 Troubleshooting S3 Events/Triggers (Lambda, SNS, SQS)
- **Issue**: Events not triggering on object actions.
- **Checks**:
  - Confirm event notification configuration.
  - Validate permissions for S3 to invoke Lambda/SNS/SQS.
- **Resolution**: Fix event rules and IAM policies.

---

## 🛠 Common CLI Commands

```bash
# List buckets
aws s3 ls

# List objects in a bucket
aws s3 ls s3://my-bucket-name/

# Upload a file
aws s3 cp file.txt s3://my-bucket-name/path/

# Download a file
aws s3 cp s3://my-bucket-name/file.txt .

# Enable versioning
aws s3api put-bucket-versioning --bucket my-bucket-name --versioning-configuration Status=Enabled
```

---

## Note:

- Always cross-check permissions before escalating.
- Use AWS Policy Simulator to validate IAM/bucket policies.
- Use CloudTrail to audit S3-related activities.
- Enable logging and versioning in critical buckets for auditing and recovery.
