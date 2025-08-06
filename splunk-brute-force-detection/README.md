Splunk-Based Home SOC Lab Project
Introduction
This project demonstrates a fully functional, miniature Security Operations Center (SOC) built at home. The lab is designed to simulate a real-world scenario where an attacker attempts to brute-force a Windows machine's Remote Desktop Protocol (RDP) service. Using Splunk as our Security Information and Event Management (SIEM) tool, this project covers the full lifecycle of an incident: ingesting logs from a victim, analyzing the data to detect malicious activity, and creating automated alerts to respond to threats in real-time.
Lab Architecture
The lab consists of three main components:
Attacker Machine: A Kali Linux VM used to launch a brute-force password attack. (IP: 192.168.154.128)
Victim Machine: A Windows 10 Pro VM that acts as the target RDP server. (IP: 192.168.154.129, Hostname: DESKTOP-2I9EIT2)
SOC / SIEM: Splunk Enterprise installed on the host machine, which collects and analyzes logs from the victim via a Splunk Universal Forwarder.
(To create your own diagram, go to draw.io, design it, export it as a PNG, upload it to a site like imgur.com, and paste the direct image link here.)
Use Case Diagram
The following diagram illustrates the interactions between the different roles (Actors) and the primary functions (Use Cases) of the Security Operations Center lab.
(Create your diagram at draw.io, export as a PNG, upload to a site like imgur.com, and paste the direct image link here.)
Actors:
Penetration Tester: Simulates the attacker by launching a brute-force attack against the victim machine.
System Administrator: Responsible for the initial setup and configuration of the lab environment, including the installation of the Splunk Forwarder and the configuration of data inputs on the Splunk server.
SOC Analyst: The defender, responsible for using the SIEM to detect, analyze, visualize, and create alerts for the malicious activity.
Use Cases:
Launch Brute-Force Attack: The act of using a tool like Hydra to systematically guess passwords for a target service.
Generate Failed Login Events: The direct result of the attack, where the Windows victim machine logs each failed attempt (Event ID 4625).
Forward & Ingest Logs: The core data pipeline where the Splunk Universal Forwarder sends logs from the victim to the Splunk Enterprise server, which then indexes the data, making it searchable.
Search, Visualize, and Alert: The workflow of the SOC Analyst, who queries the indexed data to find evidence, creates dashboards for reporting, and builds automated alerts for future incidents.
Attack Simulation & Detection
A brute-force attack was simulated using Hydra from the Kali machine against the RDP service on the Windows 10 VM.
Attack Command:
Generated bash
hydra -l Administrator -P /usr/share/wordlists/rockyou.txt rdp://192.168.154.129 -V
Use code with caution.
Bash
Detection in Splunk:
The attack was detected by searching for Windows Security Event ID 4625 (An account failed to log on). By inspecting these events, the attacker's IP address (192.168.154.128) and the targeted username (Administrator) were identified.
Splunk Query (SPL) for Analysis:
This query identifies the top attacking IP addresses by counting the number of failed logins.
Generated spl
index=* host="DESKTOP-2I9EIT2" EventCode=4625 | rename "Source Network Address" as src_ip | top limit=5 src_ip
Use code with caution.
Spl
Project Screenshots
1. Attack in Progress (Kali Terminal)
![alt text](link-to-your-image.png)
2. Detection of Failed Logins in Splunk
![alt text](link-to-your-image.png)
3. SOC Overview Dashboard
![alt text](link-to-your-image.png)
4. Real-Time Alert Configuration
![alt text](link-to-your-image.png)
Challenges & Learning
One of the primary challenges was establishing and troubleshooting the data pipeline between the Splunk Universal Forwarder and the Splunk Enterprise server. The initial connection failed, which required a systematic, multi-step diagnostic process.
Key troubleshooting steps included:
Verifying that all necessary services (splunkd on the host, SplunkForwarder on the victim) were running.
Confirming network connectivity using ping and Test-NetConnection, which led to the discovery of a third-party firewall (Avast) that was overriding the default Windows Firewall.
Creating a specific inbound rule in the Avast firewall to allow traffic on TCP port 9997 for the splunkd.exe service.
Diagnosing a missing inputs.conf file on the Universal Forwarder, which prevented any log data from being collected despite a healthy network connection. This was resolved by manually creating the file with the correct stanzas to monitor the Windows Event Logs.
Identifying the correct hostname of the victim machine (DESKTOP-2I9EIT2) for use in Splunk search queries.
This project highlighted the real-world importance of understanding not just the security tools themselves, but also the underlying network infrastructure and the often-overlooked impact of host-based security software. Overcoming these challenges provided invaluable hands-on experience in log collection, network troubleshooting, and security system configuration
