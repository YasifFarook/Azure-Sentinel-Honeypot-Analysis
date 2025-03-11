# 🔍 Analysing Real RDP Brute Force Attacks with Azure Sentinel  
📅 **Date:** March 2025  
✍️ **Author:** Yasif Farook  

## 📌 Overview  
Remote Desktop Protocol (**RDP**) is one of the most frequently targeted attack vectors by cybercriminals. Threat actors continuously scan for exposed RDP ports, launching **brute-force attacks** to gain unauthorised access.  

In this project, I deployed a **honeypot virtual machine (VM) in Azure**, capturing **real-world** RDP brute-force attack attempts. Using **Azure Sentinel**, **Log Analytics**, and **Kusto Query Language (KQL)**, I analysed attack patterns and visualised geographic attack origins.  

📂 **GitHub Repository**: [Azure Sentinel Honeypot Analysis](https://github.com/YOUR-USERNAME/Azure-Sentinel-Honeypot-Analysis)  

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
| **Event Viewer** | Logs failed login attempts (Event ID 4625) |  
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

---

## 📜 Detecting Brute-Force Attacks with KQL  
To analyse attack frequency, I created a **KQL query** in Azure Sentinel:  

```kusto
SecurityEvent
| where EventID == 4625
| summarise AttemptCount = count() by SourceIP, bin(TimeGenerated, 10m)
| where AttemptCount > 5
| project SourceIP, AttemptCount, TimeGenerated
