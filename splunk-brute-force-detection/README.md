Splunk-Based Home SOC Lab Project
Introduction
This project demonstrates a fully functional, miniature Security Operations Center (SOC) built at home. The lab is designed to simulate a real-world scenario where an attacker attempts to brute-force a Windows machine's Remote Desktop Protocol (RDP) service. Using Splunk as our Security Information and Event Management (SIEM) tool, this project covers the full lifecycle of an incident: ingesting logs from a victim, analyzing the data to detect malicious activity, and creating automated alerts to respond to threats in real-time.
Lab Architecture
The lab consists of three main components:
Attacker Machine: A Kali Linux VM used to launch a brute-force password attack. (IP: 192.168.154.128)
Victim Machine: A Windows 10 Pro VM that acts as the target RDP server. (IP: 192.168.154.129, Hostname: DESKTOP-2I9EIT2)
SOC / SIEM: Splunk Enterprise installed on the host machine, which collects and analyzes logs from the victim via a Splunk Universal Forwarder.
(Insert your architecture diagram here. You can create one easily at draw.io and upload the image to your repository.)
Attack Simulation & Detection
A brute-force attack was simulated using Hydra from the Kali machine against the RDP service on the Windows 10 VM.
Attack Command:
Generated bash
hydra -l Administrator -P /usr/share/wordlists/rockyou.txt rdp://192.168.154.129 -V
Use code with caution.
Bash
Detection in Splunk:
The attack was detected by searching for Windows Security Event ID 4625 (An account failed to log on). By inspecting these events, the attacker's IP address and the targeted username were identified.
Splunk Query (SPL) for Analysis:
Generated spl
index=* host="DESKTOP-2I9EIT2" EventCode=4625 | rename "Source Network Address" as src_ip | top limit=5 src_ip
Use code with caution.
Spl
Project Screenshots
1. Attack in Progress (Kali Terminal)
(Upload your Hydra screenshot to your GitHub repository. Then, replace this line with the following Markdown code, pasting the direct link to your image: ![Hydra Attack](link-to-your-image.png))
2. Detection of Failed Logins in Splunk
(Upload your Splunk search results screenshot. Then, replace this line with the following Markdown code, pasting the direct link to your image: ![Splunk Detection](link-to-your-image.png))
3. SOC Overview Dashboard
(Upload your Splunk dashboard screenshot. Then, replace this line with the following Markdown code, pasting the direct link to your image: ![SOC Dashboard](link-to-your-image.png))
4. Real-Time Alert Configuration
(Upload your Splunk alert configuration screenshot. Then, replace this line with the following Markdown code, pasting the direct link to your image: ![Alert Configuration](link-to-your-image.png))
Challenges & Learning
One of the primary challenges was establishing and troubleshooting the data pipeline between the Splunk Universal Forwarder and the Splunk Enterprise server. The initial connection failed, which required a systematic, multi-step diagnostic process.
Key troubleshooting steps included:
Verifying that all necessary services (splunkd on the host, SplunkForwarder on the victim) were running.
Confirming network connectivity using ping and Test-NetConnection, which led to the discovery of a third-party firewall (Avast) that was overriding the default Windows Firewall.
Creating a specific inbound rule in the Avast firewall to allow traffic on TCP port 9997 for the splunkd.exe service.
Diagnosing a missing inputs.conf file on the Universal Forwarder, which prevented any log data from being collected despite a healthy network connection.
Identifying the correct hostname of the victim machine for use in Splunk search queries.
This project highlighted the real-world importance of understanding not just the security tools themselves, but also the underlying network infrastructure and the often-overlooked impact of host-based security software. Overcoming these challenges provided invaluable hands-on experience in log collection, network troubleshooting, and security system configuration.
