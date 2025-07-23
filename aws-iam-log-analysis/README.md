# AWS Project: IAM Misconfiguration Detection using CloudTrail Logs

## 🔧 Project Overview
Monitor AWS CloudTrail logs to detect risky IAM misconfigurations, such as overly permissive policies (`*:*`), unknown user creation, or role changes. This simulates a real-world cloud security analyst scenario.

---

## ⚖️ Tools Used
- AWS Free Tier
- CloudTrail (enabled in default region)
- IAM (users, groups, policies)
- AWS Console or Athena (for log queries)

---

## 📝 Project Goals
- Understand and enable AWS CloudTrail
- Generate IAM events: create user, attach policy, etc.
- Review logs for suspicious or misconfigured actions
- Detect wildcard permissions (`Action: *`, `Resource: *`)

---

## ☁️ Steps Followed

### 1. Enable CloudTrail
- Go to AWS Console → CloudTrail
- Enable **management events**
- Create a new trail with S3 bucket logging

### 2. Create Misconfig Events
- Create new IAM user via console or CLI
- Attach a custom inline policy like:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

### 3. Search Logs for Risky Events
Search CloudTrail logs in **Athena** or **CloudTrail Event History**:
- `CreateUser`
- `AttachUserPolicy`
- `PutUserPolicy`
- `CreateAccessKey`
- `UpdateAssumeRolePolicy`

---

## 🌐 Sample Log Indicators
- **Event Name**: `PutUserPolicy`
- **PolicyDocument** contains: `"Action":"*", "Resource":"*"`
- **UserIdentity**: unexpected or unauthorized user

---

## 💡 Security Outcome
- Identified overly permissive IAM policies
- Flagged risky access key creations and policy changes
- Practiced real-life cloud log triage

---

## 💳 Resume Bullet Point
> ✓ Detected and analyzed IAM misconfigurations in AWS using CloudTrail logs to identify wildcard permissions and suspicious access changes.

---

## 📷 Screenshot Placeholder
*Upload screenshot of CloudTrail event or policy JSON here.*

---

## 🚀 Future Ideas
- Automate detection with AWS Config or Lambda
- Use CloudWatch Metric Filters for real-time alerting
- Integrate findings into SIEM or Splunk

---

## 📈 What I Learned
- IAM structure: users, policies, roles
- CloudTrail log structure and key fields
- Practical detection of common cloud misconfigs

---
