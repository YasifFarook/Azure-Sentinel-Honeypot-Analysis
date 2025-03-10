# Azure Sentinel Honeypot Threat Detection  
### 📌 Detecting and analysing real-world RDP brute-force attacks using Azure Sentinel  
**Author:** Yasif Farook  
**Date:** March 2025  

## 🔍 Project Overview  
This project investigates real RDP brute-force attacks using a **honeypot VM** in Azure. Attack data is collected via **Event Viewer**, enriched with **geolocation**, and analysed in **Azure Sentinel** using KQL queries.

## 🛠 Technologies Used  
- **Azure Sentinel** for SIEM analysis  
- **Log Analytics & KQL** for threat detection  
- **Windows Server (Honeypot VM)** for real-world attack logging  
- **Geolocation API** for threat attribution  
- **PowerShell** for log automation  

## 📊 Key Findings  
- Thousands of failed RDP login attempts recorded.  
- Most attacks originated from known **botnets** and **credential-stuffing campaigns**.  
- Real-world attack data visualized on a **global heatmap** in Sentinel.  

## 🚀 How to Use This Project  
1. Deploy a honeypot VM in Azure.  
2. Enable logging & connect to **Azure Log Analytics**.  
3. Use **KQL queries** to detect attacks.  
4. Visualize results in **Sentinel’s Map View**.  

## 📎 Files in This Repository  
- `Honeypot-Based Threat Detection_ Real-World Attack Analysis with Azure Sentinel by Yasif Farook` - Full project report
- `KQL_Query_to_Detect_Activity.kql` - KQL query for detecting brute force attacks
- `Sentinel Powershell Script` - PowerShell script for log automation
- `failed_rdp.log` - Attack logs
- `honeypot-vm details` - Honeypot VM details to attempt logins and monitor the SIEM accordingly

## 📬 Connect with Me  
🔗 **[LinkedIn](https://www.linkedin.com/in/yasif-farook-ab991a22b/)**  
📧 yasfarook937@gmail.com  
