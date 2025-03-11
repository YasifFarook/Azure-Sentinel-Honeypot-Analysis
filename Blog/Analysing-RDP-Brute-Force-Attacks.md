# 🔍 Analysing Real RDP Brute Force Attacks with Azure Sentinel  
📅 **Date:** March 2025  
✍️ **Author:** Yasif Farook  

## 📌 Overview  
Remote Desktop Protocol (**RDP**) is one of the most frequently targeted attack vectors by cybercriminals. Threat actors continuously scan for exposed RDP ports, launching **brute-force attacks** to gain unauthorised access.  

In this project, I deployed a **honeypot virtual machine (VM) in Azure**, capturing **real-world** RDP brute-force attack attempts. Using **Azure Sentinel**, **Log Analytics**, and **Kusto Query Language (KQL)**, I analysed attack patterns and visualised geographic attack origins.  

📂 **GitHub Repository**: [Azure Sentinel Honeypot Analysis](https://github.com/YasifFarook/Azure-Sentinel-Honeypot-Analysis)  

---

## 🚀 Why I Built This Project  
As a cybersecurity student aiming to become a **SOC Analyst**, I wanted hands-on experience with **real attack data**, not just simulated threats. This project helped me:  

✅ **Detect & analyse brute-force attempts** in real time  
✅ **Use Azure Sentinel** for security monitoring  
✅ **Create KQL queries** to detect attack patterns  
✅ **Enrich logs with geolocation data**  

---

## 🛠 Technology Stack  
| Technology | Purpose |  
|------------|---------|  
| **Azure Sentinel** | SIEM (Security Information and Event Management) for real-time threat detection |  
| **Windows Server VM** | Honeypot to attract real-world RDP attacks |  
| **Event Viewer** | Logs event activity including failed login attempts (Event ID 4625) |  
| **Azure Log Analytics** | Stores and queries log data |  
| **Kusto Query Language (KQL)** | Analyses attack logs |  
| **IP Geolocation API** | Identifies the geographic origin of attack attempts |  

---

## 📊 Capturing Real Attack Data  
Instead of relying on simulated attacks, I deployed an **Azure Windows Server VM** with **public RDP access (port 3389 open)** to attract attackers.  

### 🔹 Key Setup Details:  
- Assigned a **static public IP**  
- Configured **weak credentials** to encourage brute-force attempts  
- Enabled **detailed Windows logging** for authentication events  

Within **hours of deployment**, the honeypot recorded **thousands of failed login attempts**, originating from multiple global IP addresses.

![](https://github.com/YasifFarook/Azure-Sentinel-Honeypot-Analysis/blob/master/Log_Activity_on_PowerShell.png)

---

## 📜 Detecting Brute-Force Attacks with KQL  
To analyse attack frequency, I created a **KQL query** in Azure Sentinel.
I used the FAILED_RDP_WITH_GEO_CL table, which stores logs related to failed RDP login attempts. This query extracts key details such as the username, source IP, and geolocation data::  

```
FAILED_RDP_WITH_GEO_CL
| extend Timestamp = todatetime(TimeGenerated),
         Latitude = tostring(extract("latitude:([0-9.-]+)", 1, RawData)),
         Longitude = tostring(extract("longitude:([0-9.-]+)", 1, RawData)),
         DestinationHost = tostring(extract("destinationhost:([\\w.-]+)", 1, RawData)),
         Username = tostring(extract("username:([\\w.-]+)", 1, RawData)),
         SourceHost = tostring(extract("sourcehost:([\\d.]+)", 1, RawData)),
         State = tostring(extract("state:([\\w\\s]+)", 1, RawData)),
         Country = tostring(extract("country:([\\w\\s]+)", 1, RawData)),
         Label = tostring(extract("label:([\\w\\s.-]+)", 1, RawData))
| project Timestamp, Username, SourceHost, DestinationHost, State, Country, Latitude, Longitude, Label
```

💡 Insights from Query Results:
* Extracted IP addresses of attackers.
* Parsed geolocation details to identify high-risk regions.
* Filtered failed login attempts based on timestamp and attack source.


🌍 Mapping Attack Sources Using Geolocation Data
To trace the origin of attacks, I enriched logs with geolocation API data, mapping each IP address to its country and coordinates.

📌 Enhanced KQL Query for Geolocation:

```
FAILED_RDP_WITH_GEO_CL
| summarize EventCount = count() by Country
| sort by EventCount desc
```
This allowed Sentinel to plot attack origins on a world map!📍

![Sentinel Map View](https://github.com/YasifFarook/Azure-Sentinel-Honeypot-Analysis/blob/master/Sentinel_Global_Map.png?raw=true)

🔹 Findings:

 - An overwhelming majority of attack traffic came from The Netherlands.

 - Some attacking IPs were already listed in threat intelligence databases as part of botnets.

 - The attack volume indicated an automated campaign attempting credential stuffing.
   
📌 Key Findings & SOC Analyst Takeaways:

📈 Attack Volume: The honeypot logged thousands of failed login attempts within 24 hours.

🤖 Botnet Indicators: Many IPs followed a similar attack pattern, suggesting use of automated scripts.

🌍 Geo-Attribution: Attackers originated from diverse global locations, reinforcing the need for geo-blocking in RDP security.

🔍 SOC Strategy: This data could be correlated with known threats for improved incident response.


<h2>🔧 How This Project Strengthens My SOC Analyst Skills</h2>

This project helped me build core cybersecurity skills, including:

✅ Threat Hunting: Using KQL to analyse attack logs

✅ SIEM Operations: Configuring Azure Sentinel for security monitoring

✅ Geolocation Enrichment: Identifying high-risk attack sources

✅ Incident Response: Detecting brute-force attempts in real time

<h3>🚀 Next Steps: Enhancing This Project</h3>
To improve detection and response, I plan to:

<br>🔹 Automate detection & response using Sentinel Playbooks</br>

🔹 Correlate logs across multiple honeypots to track global threat actors

🔹 Integrate Threat Intelligence Feeds to check if attacker IPs are already known

<h2>📌 Conclusion: Why This Matters</h2>

This project proves that RDP remains a top attack vector, and unprotected systems are quickly targeted by real-world attackers.

💡 Real-world attacks require real-world defences—SIEM solutions like Azure Sentinel provide the visibility needed to detect and mitigate these threats.

📂 Full Project Repo: [Azure Sentinel Honeypot Analysis](https://github.com/YasifFarook/Azure-Sentinel-Honeypot-Analysis)

💬 Let’s Discuss: Have you worked with Sentinel, KQL, or SOC operations? Let’s connect on [LinkedIn!](https://www.linkedin.com/in/yasif-farook-ab991a22b/)
