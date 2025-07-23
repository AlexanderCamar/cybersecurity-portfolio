# Splunk Project: Brute Force Attack Detection

## 🔧 Project Overview
Detect brute-force login attempts using Splunk by analyzing authentication logs. The project simulates how a SOC Analyst monitors and creates alerts for suspicious login activity.

---

## ⚖️ Tools Used
- Splunk Enterprise (or Splunk Cloud Trial)
- Sample auth logs (`linux_secure.log` or Windows Event Logs)
- SPL (Search Processing Language)

---

## 📝 Project Goals
- Ingest authentication logs into Splunk
- Create SPL query to detect repeated failed login attempts
- Build alert for brute-force patterns (e.g. >5 failed logins in 5 mins)
- Design a simple dashboard to visualize suspicious IPs

---

## 💡 SPL Query for Brute Force Detection
```spl
index=auth sourcetype=linux_secure "Failed password" \
| stats count by src_ip \
| where count > 5
```

### Explanation:
- Searches for "Failed password" strings
- Groups by source IP
- Filters only IPs with more than 5 failed attempts

---

## 🔔 Create an Alert
- Navigate to **Save As → Alert**
- Trigger condition: `if number of results > 0`
- Time range: Last 5 mins (or 15 mins)
- Alert Action: Email or trigger script

---

## 📊 Create Dashboard Panel
```spl
index=auth sourcetype=linux_secure "Failed password" \
| stats count by src_ip \
| sort -count
```
- Save as Pie Chart or Bar Graph
- Add panel to new Dashboard: `Brute Force Monitoring`

---

## 📈 Outcome
- Alert triggered when brute force attempts occur
- Dashboard shows top offending IPs
- Demonstrates practical SOC task

---

## 🎓 What I Learned
- How to write SPL queries
- How to build alerts and dashboards in Splunk
- Understanding brute-force indicators in log data

---

## 🗋 Resume Bullet Point
> ✓ Created real-time Splunk alert and dashboard to detect brute force attacks from auth logs using SPL.

---

## 📷 Screenshot Placeholder
*Upload your real Splunk alert screenshot here once built*

---


