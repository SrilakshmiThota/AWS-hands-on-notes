# 📦 Project: Host HTTP Server on EC2 and Auto-Upload Apache Logs to S3

## 🔧 Overview
This project demonstrates how to:
- Launch an Apache HTTP server on an Amazon EC2 instance.
- Collect real-time Apache access and error logs.
- Automatically upload the logs to an S3 bucket using a bash script and `cron`.

---

## 🛠️ Tech Stack
- **Amazon EC2** – Linux-based virtual machine  
- **Apache HTTPD** – Web server  
- **Amazon S3** – Object storage for storing logs  
- **Bash Script** – To automate log upload  
- **Crontab** – To schedule log uploads every 10 minutes  
- **AWS CLI** – To interact with S3 from the EC2 instance

---

## 🚀 Step-by-Step Setup

### ✅ Step 1: Launch EC2 Instance
- Launch Amazon Linux 2 EC2 instance.
- Open the following ports in **Security Group**:
  - SSH (22) → **Your IP**
  - HTTP (80) → **0.0.0.0/0**

---

### ✅ Step 2: Install Apache HTTPD
```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

---

### ✅ Step 3: Test HTTP Server
Create an `index.html` file:
```bash
echo "It Works!" | sudo tee /var/www/html/index.html
```

Access the public IP in your browser to verify:
```
http://<your-ec2-public-ip>
```

---

### ✅ Step 4: Create S3 Bucket
- Go to AWS S3 Console
- Create a bucket (e.g., `my-httpd-logs-bucket-fromec2`)
- Make sure EC2 has **S3 access** via IAM Role or configure credentials using:
```bash
aws configure
```

---

### ✅ Step 5: Write the Bash Script
Create the file:
```bash
sudo nano /home/ec2-user/upload_logs.sh
```

Paste the following:
```bash
#!/bin/bash

TIMESTAMP=$(date +"%Y-%m-%d-%H-%M-%S")
ACCESS_LOG="/var/log/httpd/access_log"
ERROR_LOG="/var/log/httpd/error_log"
S3_BUCKET="s3://my-httpd-logs-bucket-fromec2/logs"

aws s3 cp $ACCESS_LOG $S3_BUCKET/access_log_$TIMESTAMP.log
aws s3 cp $ERROR_LOG $S3_BUCKET/error_log_$TIMESTAMP.log
```

Make it executable:
```bash
chmod +x /home/ec2-user/upload_logs.sh
```

Test manually:
```bash
/home/ec2-user/upload_logs.sh
```

---

### ✅ Step 6: Install & Configure Crontab
Install cron:
```bash
sudo yum install cronie -y
sudo systemctl enable crond
sudo systemctl start crond
```

Edit crontab:
```bash
crontab -e
```

Add this line to schedule the script every 10 minutes:
```bash
*/10 * * * * /home/ec2-user/upload_logs.sh
```

---

### ✅ Step 7: Verify Upload
Check the uploaded logs in S3:
```bash
aws s3 ls s3://my-httpd-logs-bucket-fromec2/logs/
```

---

## 📁 Folder Structure
```
/
├── upload_logs.sh        # Bash script to upload logs to S3
├── /var/www/html         # Apache root directory
│   └── index.html        # Default test page
```

---

## 🧼 Optional Enhancements
- Enable **S3 Lifecycle Policy** to auto-delete logs after X days
- Configure **CloudWatch Alerts** on failures
- Extend script to include log rotation or compression
- Write logs to a custom S3 folder per day (`logs/yyyy-mm-dd/`)

---

## 🧠 Learning Outcome
- Hosting and securing Apache on EC2
- Managing Linux permissions, services, and scheduling
- Automating AWS tasks using shell scripting
- Integrating EC2 and S3 for real-world use cases
