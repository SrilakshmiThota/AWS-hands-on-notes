# Task 2: AWS Resource Report Script - README

## 🔖 Objective

Automatically fetch AWS resources (EC2, S3, IAM, Security Groups, Key Pairs, and CloudWatch Alarms) from specified regions and export them into an Excel file stored in OneDrive.

---

## 📁 Project Structure

```
aws_resource_report/
├── updated_fetch_aws_resources.py     # Python script to collect AWS resources
└── README.md                          # Documentation file (this file)
```

---

## ✅ Prerequisites

- Python 3.x installed
- Virtual environment (recommended)
- AWS CLI configured using `aws configure`
- IAM user credentials with permissions to describe/list resources (EC2, S3, IAM, CloudWatch)
- OneDrive folder correctly mounted/synced on the local Linux system

---

## ⚖️ Libraries Used

- **boto3**: AWS SDK for Python
- **pandas**: Data manipulation and export to Excel
- **datetime** and **pytz**: For timestamp formatting and handling

---

## 🔄 Execution Steps

### 1. Clone or create project directory:

```bash
mkdir aws_resource_report
cd aws_resource_report
```

### 2. Create virtual environment and activate:

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies:

```bash
pip install boto3 pandas openpyxl pytz
```

### 4. Save the following as `updated_fetch_aws_resources.py`:

```python
import boto3
import pandas as pd
from datetime import datetime
import pytz

regions = ['ap-south-1', 'us-east-1']
timestamp = datetime.now().strftime("%Y-%m-%d_%H-%M-%S")
excel_path = f'/home/srilakshmi/OneDrive/aws_resources_report_{timestamp}.xlsx'

ec2_data = []
s3_data = []
iam_data = []
sg_data = []
keypair_data = []
cw_data = []

def remove_timezone(dt):
    return dt.replace(tzinfo=None) if dt and dt.tzinfo else dt

def collect_ec2(region):
    ec2 = boto3.client('ec2', region_name=region)
    response = ec2.describe_instances()
    for reservation in response['Reservations']:
        for instance in reservation['Instances']:
            ec2_data.append({
                'Region': region,
                'Instance ID': instance['InstanceId'],
                'State': instance['State']['Name'],
                'Type': instance['InstanceType'],
                'AZ': instance['Placement']['AvailabilityZone'],
                'Launch Time': remove_timezone(instance['LaunchTime']),
                'CPU Options': str(instance.get('CpuOptions')),
                'Key Name': instance.get('KeyName', ''),
                'Public IP': instance.get('PublicIpAddress', ''),
                'Private IP': instance.get('PrivateIpAddress', ''),
                'Security Groups': ", ".join([sg['GroupName'] for sg in instance['SecurityGroups']])
            })

def collect_s3():
    s3 = boto3.client('s3')
    response = s3.list_buckets()
    for bucket in response['Buckets']:
        name = bucket['Name']
        try:
            region = s3.get_bucket_location(Bucket=name)['LocationConstraint']
            region = region or 'us-east-1'
            objects = boto3.client('s3', region_name=region).list_objects_v2(Bucket=name)
            total_objects = objects.get('KeyCount', 0)
        except:
            region = 'unknown'
            total_objects = 'Error'
        s3_data.append({
            'Bucket Name': name,
            'Creation Date': remove_timezone(bucket['CreationDate']),
            'Region': region,
            'Total Objects': total_objects
        })

def collect_iam():
    iam = boto3.client('iam')
    users = iam.list_users()['Users']
    for user in users:
        policies = iam.list_attached_user_policies(UserName=user['UserName'])['AttachedPolicies']
        policy_names = ", ".join([p['PolicyName'] for p in policies])
        iam_data.append({
            'User Name': user['UserName'],
            'Created On': remove_timezone(user['CreateDate']),
            'Policies': policy_names
        })

def collect_sg(region):
    ec2 = boto3.client('ec2', region_name=region)
    sgs = ec2.describe_security_groups()['SecurityGroups']
    for sg in sgs:
        sg_data.append({
            'Region': region,
            'Group Name': sg['GroupName'],
            'Group ID': sg['GroupId'],
            'Description': sg['Description'],
            'VPC ID': sg.get('VpcId', '')
        })

def collect_keypairs(region):
    ec2 = boto3.client('ec2', region_name=region)
    keys = ec2.describe_key_pairs()['KeyPairs']
    for key in keys:
        keypair_data.append({
            'Region': region,
            'Key Name': key['KeyName'],
            'Key Fingerprint': key['KeyFingerprint']
        })

def collect_cloudwatch(region):
    cw = boto3.client('cloudwatch', region_name=region)
    alarms = cw.describe_alarms()['MetricAlarms']
    for alarm in alarms:
        cw_data.append({
            'Region': region,
            'Alarm Name': alarm['AlarmName'],
            'Metric': alarm['MetricName'],
            'Namespace': alarm['Namespace'],
            'State': alarm['StateValue'],
            'Created': remove_timezone(alarm['StateUpdatedTimestamp'])
        })

def collect_all_resources():
    for region in regions:
        collect_ec2(region)
        collect_sg(region)
        collect_keypairs(region)
        collect_cloudwatch(region)
    collect_s3()
    collect_iam()

    with pd.ExcelWriter(excel_path) as writer:
        if ec2_data:
            pd.DataFrame(ec2_data).to_excel(writer, sheet_name='EC2', index=False)
        if s3_data:
            pd.DataFrame(s3_data).to_excel(writer, sheet_name='S3', index=False)
        if iam_data:
            pd.DataFrame(iam_data).to_excel(writer, sheet_name='IAM', index=False)
        if sg_data:
            pd.DataFrame(sg_data).to_excel(writer, sheet_name='SecurityGroups', index=False)
        if keypair_data:
            pd.DataFrame(keypair_data).to_excel(writer, sheet_name='KeyPairs', index=False)
        if cw_data:
            pd.DataFrame(cw_data).to_excel(writer, sheet_name='CloudWatch', index=False)

    print(f"\u2705 AWS resource report saved to: {excel_path}")

if __name__ == "__main__":
    collect_all_resources()
```

---

## 📅 Output

- Excel file is generated at: `/home/srilakshmi/OneDrive/aws_resources_report_<timestamp>.xlsx`
- File is automatically synced to OneDrive

---

## 🚀 Run Command

```bash
python updated_fetch_aws_resources.py
```

---

## 📃 Author

Sri Lakshmi

