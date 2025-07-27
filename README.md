# AWS Resource Fetcher & Excel Reporter (Synced to OneDrive)

## 🧠 Overview

This Python script fetches AWS resource details across multiple regions (EC2, S3, IAM, CloudWatch alarms, RDS, etc.) using Boto3 and saves the information into an Excel file. The report is stored in a OneDrive-synced folder, allowing you to access it anywhere.

---

## ✅ Features

- Fetches resource data from:
  - EC2 Instances
  - IAM Users and Policies
  - S3 Buckets and their Objects
  - CloudWatch Alarms
  - RDS Instances
- Outputs all data into different Excel sheets
- Generates a timestamped Excel file (e.g., `aws_resources_report_2025-07-17_14-30-55.xlsx`)
- Automatically stores file in your OneDrive directory (e.g., `/home/username/OneDrive/`)
- Supports multiple AWS regions

---

## 📁 Directory Structure

```
aws_resource_report/
├── updated_fetch_resources.py
├── README.md
└── venv/
```

---

## ⚙️ Prerequisites

- Python 3.x installed
- Boto3 and Pandas installed:
  ```bash
  pip install boto3 pandas openpyxl
  ```
- AWS CLI configured using:
  ```bash
  aws configure
  ```
- OneDrive must be set up and synced locally

---

## 🚀 How to Run

1. **Activate Virtual Environment (if any):**
   ```bash
   source venv/bin/activate
   ```

2. **Run the script:**
   ```bash
   python updated_fetch_resources.py
   ```

3. **Check OneDrive for the Excel report file:**
   - File name will be like: `aws_resources_report_2025-07-17_14-30-55.xlsx`
   - Location: `/home/username/OneDrive/`

---

## 🧾 Output

- A multi-sheet Excel file containing AWS resource data.
- Example sheets:
  - `EC2_ap-south-1`
  - `IAM_Users`
  - `S3_Buckets`
  - `CloudWatch_ap-south-1`
  - `RDS_ap-south-1`

---

## 🌍 Supported AWS Regions

Modify the `regions` list inside the script to include the regions you want:
```python
regions = ['ap-south-1', 'us-east-1']
```

---

## 🔒 Security

Make sure your AWS credentials are not hardcoded. Use `~/.aws/credentials` managed via `aws configure`.

---

## 👩‍💻 Author

- **T Srilakshmi**
- Task Owner: AWS Hands-On Automation Task 2
- Goal: Master AWS resource management and reporting automation

---

## 📝 License

This project is for learning purposes under the MIT License.