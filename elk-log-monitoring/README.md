# ELK Stack Project: Detecting Failed SSH Logins

## 🔧 Project Overview
Use the ELK Stack (Elasticsearch, Logstash, Kibana) to monitor log files for failed SSH login attempts. This simulates a basic but powerful real-world use case in blue team SOC environments.

---

## ⚖️ Tools Used
- Elastic Stack (ELK)
  - Elasticsearch
  - Logstash
  - Kibana
- Filebeat (for log forwarding)
- Sample Linux auth logs (`/var/log/auth.log`)

---

## 📝 Project Goals
- Configure Filebeat to forward logs to Logstash
- Parse failed SSH login entries
- Visualize top source IPs of failed logins
- Build Kibana dashboard and alert

---

## ⚡ Setup Process

### 1. Install ELK Stack (Local or Docker)
```bash
sudo apt update && sudo apt install elasticsearch logstash kibana filebeat
```
Or use Docker Compose stack.

### 2. Configure Filebeat
Edit `/etc/filebeat/filebeat.yml`:
```yaml
filebeat.inputs:
  - type: log
    paths:
      - /var/log/auth.log
output.logstash:
  hosts: ["localhost:5044"]
```

### 3. Configure Logstash Pipeline
```bash
input {
  beats {
    port => 5044
  }
}

filter {
  grok {
    match => { "message" => "%{SYSLOGTIMESTAMP:timestamp} %{DATA:source} sshd\[%{NUMBER}\]: Failed password for %{DATA:user} from %{IP:src_ip}.*" }
  }
}

output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "ssh-failed-logins"
  }
}
```

### 4. Start Services
```bash
sudo service elasticsearch start
sudo service logstash start
sudo service kibana start
sudo filebeat modules enable system
sudo filebeat setup
sudo service filebeat start
```

---

## 📈 Kibana Dashboard Idea
- Bar chart: Count of failed logins by IP
- Table: Users targeted with failed logins
- Line graph: Login failure trend over time

---

## 🔔 Optional Alert Setup
Use **Kibana → Stack Management → Alerts & Actions** to create:
- Threshold rule: If count of `Failed password` from same IP > 5 in 5 mins
- Alert via email or Slack

---

## 🚀 Outcome
- Identified SSH brute-force attempts
- Built log ingestion pipeline with Filebeat + Logstash
- Created dashboard showing real-time attack attempts

---

## 💳 Resume Bullet Point
> ✓ Built ELK Stack pipeline to detect failed SSH login attempts, visualized in Kibana dashboard and alert system.

---

## 📷 Screenshot Placeholder
*Upload Kibana dashboard screenshot or alert rule example here*

---

## ✅ What I Learned
- ELK Stack architecture (beats → logstash → elasticsearch → kibana)
- How to write Grok patterns
- Linux log structure (auth.log parsing)
- How log pipelines help blue team visibility

---
