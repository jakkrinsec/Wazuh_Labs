# การตรวจจับการบุกรักและการตอบสนองด้วย Wazuh
## Introduction
โปรเจ็คนี้สร้างเพื่อทดสอบการตรวจจับการบุกรุกและการวิเคราะห์วิธีการด้วย Wazuh
## Setup
### 1. Requirement of Wazuh
- Red Hat Enterprise Linux 7, 8, 9; 
- CentOS 7, 8; 
- Amazon Linux 2; 
- Ubuntu 16.04, 18.04, 20.04, 22.04.
โดยเราจะเลือก ติดตั้งบน Ubantu 22.04
### 1. ติดตั้ง Ubantu 22.04 บน Virtual Box
- ดาวน์โหลด Ubuntu Server 20.04 LTS ISO จาก https://ubuntu.com/download/server
- เริ่มต้น VM และเลือกไฟล์ ISO ที่ดาวน์โหลดเป็น optical drive
- ทำตามขั้นตอนการติดตั้ง Ubuntu Server (เลือกติดตั้ง OpenSSH Server ด้วย)
- หลังติดตั้งเสร็จ ล็อกอินและอัปเดตระบบ
``` bash
sudo apt update && sudo apt upgrade -y
```
### 2. ติดตั้ง Wazuh Server
  #### 2.1 ติดตั้งเครื่องมือที่จำเป็น
  ``` bash
  sudo apt install curl apt-transport-https unzip wget gnupg -y
  ```
  
  #### 2.2 ดาวน์โหลดและรัน Wazuh Installation Assistant
  ``` bash
  # ดาวน์โหลดสคริปต์ติดตั้ง
  curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh

  # ให้สิทธิ์ในการรันสคริปต์
  chmod +x wazuh-install.sh

  # รันสคริปต์ติดตั้งแบบ All-in-one (Wazuh Server + Indexer + Dashboard)
  sudo ./wazuh-install.sh -a
  ```
  
### 3 ล็อคอิน Wazuh Dashboard
  - เปิดเว็บบราวเซอร์บนเครื่องโฮสต์
  - เข้าถึง https://[IP-ของ-VM]
  - ยอมรับความเสี่ยงเกี่ยวกับใบรับรองที่ลงนามด้วยตนเอง (self-signed certificate)
  - ล็อกอินด้วยข้อมูลที่แสดงในเทอร์มินัลหลังการติดตั้ง
  ``` bash
  29/04/2025 11:15:55 INFO: --- Summary ---
  29/04/2025 11:15:55 INFO: You can access the web interface https://<wazuh-dashboard-ip>:443
      User: admin
      Password: 5ED5fSka*2nE5?xw6?gE7Yuw8NT2H.il
  29/04/2025 11:15:55 INFO: Installation finished
  ```
  หน้าเว็บ Login
  ![image](https://github.com/user-attachments/assets/34bdbd6d-d204-4ee2-88a0-de6b6471eb22)

### 4 การติดตั้ง Wazuh Agent บน Windows 10
 - ไปที่ Wazuh Dashboard > Agents > Deploy new agent และทำตามคำแนะนำ
  ![image](https://github.com/user-attachments/assets/41bc29c4-ecb8-4102-bdfb-612eaa1a25e1)
  ![image](https://github.com/user-attachments/assets/5e0fd02f-9114-4b1c-b0b9-c989c5b77169)

### 5 ตรวจสอบการเชื่อมต่อ Agent
 - ใน Wazuh Dashboard ไปที่ "Agents"
 - ตรวจสอบว่า Agent ที่ติดตั้งแสดงสถานะ "Active"
  ![image](https://github.com/user-attachments/assets/8ef04b00-63cf-4746-a6b1-e6a409cbe446)

 
รองรับการติดตั้งบน
Red Hat Enterprise Linux 7, 8, 9; 
CentOS 7, 8; 
Amazon Linux 2; 
Ubuntu 16.04, 18.04, 20.04, 22.04.
โดยเราจะเลือก ติดตั้งบน Ubantu 22.04





หลังจากติดตั้งเสร็จ จะได้ Username & Password มา
29/04/2025 11:11:43 INFO: Starting Wazuh installation assistant. Wazuh version: 4.7.5
29/04/2025 11:11:43 INFO: Verbose logging redirected to /var/log/wazuh-install.log
29/04/2025 11:11:49 INFO: Wazuh web interface port will be 443.
29/04/2025 11:11:55 INFO: Wazuh repository added.
29/04/2025 11:11:55 INFO: --- Configuration files ---
29/04/2025 11:11:55 INFO: Generating configuration files.
29/04/2025 11:11:56 INFO: Created wazuh-install-files.tar. It contains the Wazuh cluster key, certificates, and passwords necessary for installation.
29/04/2025 11:11:56 INFO: --- Wazuh indexer ---
29/04/2025 11:11:56 INFO: Starting Wazuh indexer installation.
29/04/2025 11:13:04 INFO: Wazuh indexer installation finished.
29/04/2025 11:13:04 INFO: Wazuh indexer post-install configuration finished.
29/04/2025 11:13:04 INFO: Starting service wazuh-indexer.
29/04/2025 11:13:15 INFO: wazuh-indexer service started.
29/04/2025 11:13:15 INFO: Initializing Wazuh indexer cluster security settings.
29/04/2025 11:13:26 INFO: Wazuh indexer cluster initialized.
29/04/2025 11:13:26 INFO: --- Wazuh server ---
29/04/2025 11:13:26 INFO: Starting the Wazuh manager installation.
29/04/2025 11:14:10 INFO: Wazuh manager installation finished.
29/04/2025 11:14:10 INFO: Starting service wazuh-manager.
29/04/2025 11:14:25 INFO: wazuh-manager service started.
29/04/2025 11:14:25 INFO: Starting Filebeat installation.
29/04/2025 11:14:30 INFO: Filebeat installation finished.
29/04/2025 11:14:31 INFO: Filebeat post-install configuration finished.
29/04/2025 11:14:31 INFO: Starting service filebeat.
29/04/2025 11:14:32 INFO: filebeat service started.
29/04/2025 11:14:32 INFO: --- Wazuh dashboard ---
29/04/2025 11:14:32 INFO: Starting Wazuh dashboard installation.
29/04/2025 11:15:36 INFO: Wazuh dashboard installation finished.
29/04/2025 11:15:36 INFO: Wazuh dashboard post-install configuration finished.
29/04/2025 11:15:36 INFO: Starting service wazuh-dashboard.
29/04/2025 11:15:37 INFO: wazuh-dashboard service started.
29/04/2025 11:15:54 INFO: Initializing Wazuh dashboard web application.
29/04/2025 11:15:55 INFO: Wazuh dashboard web application initialized.
29/04/2025 11:15:55 INFO: --- Summary ---
29/04/2025 11:15:55 INFO: You can access the web interface https://<wazuh-dashboard-ip>:443
    User: admin
    Password: 5ED5fSka*2nE5?xw6?gE7Yuw8NT2H.il
29/04/2025 11:15:55 INFO: Installation finished.


![image](https://github.com/user-attachments/assets/34bdbd6d-d204-4ee2-88a0-de6b6471eb22)

ขั้นตอนที่ 4: ติดตั้ง Wazuh Agent บนเครื่องโฮสต์
สำหรับ Windows
ไปที่ Wazuh Dashboard > Agents > Deploy new agent
เลือก Windows และทำตามคำแนะนำสำหรับการดาวน์โหลดและติดตั้ง
![image](https://github.com/user-attachments/assets/31f00d7e-8229-44e1-895e-424316baa8f4)

![image](https://github.com/user-attachments/assets/41bc29c4-ecb8-4102-bdfb-612eaa1a25e1)
![image](https://github.com/user-attachments/assets/5e0fd02f-9114-4b1c-b0b9-c989c5b77169)



PS C:\WINDOWS\system32> Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.5-1.msi -OutFile ${env.tmp}\wazuh-agent; msiexec.exe /i ${env.tmp}\wazuh-agent /q WAZUH_MANAGER='192.168.1.102' WAZUH_AGENT_NAME='My_PC' WAZUH_REGISTRATION_SERVER='192.168.1.102'
PS C:\WINDOWS\system32>
PS C:\WINDOWS\system32>
PS C:\WINDOWS\system32> NET START WazuhSvc
The Wazuh service is starting.
The Wazuh service was started successfully.


เช็คการ Connect ของ My_PC
jakkrinsec@WazuhServer:~$ sudo tail -f /var/ossec/logs/ossec.log
[sudo] password for jakkrinsec:
2025/04/29 11:14:22 sca: INFO: Starting evaluation of policy: '/var/ossec/ruleset/sca/cis_ubuntu22-04.yml'
2025/04/29 11:14:22 wazuh-modulesd:syscollector: INFO: Evaluation finished.
2025/04/29 11:14:26 sca: INFO: Evaluation finished for policy '/var/ossec/ruleset/sca/cis_ubuntu22-04.yml'
2025/04/29 11:14:26 sca: INFO: Security Configuration Assessment scan finished. Duration: 4 seconds.
2025/04/29 11:14:47 rootcheck: INFO: Ending rootcheck scan.
2025/04/29 11:44:36 wazuh-authd: INFO: New connection from 192.168.1.101
2025/04/29 11:44:36 wazuh-authd: INFO: Received request for a new agent (My_PC) from: 192.168.1.101
2025/04/29 11:44:36 wazuh-authd: INFO: Agent key generated for 'My_PC' (requested by any)
2025/04/29 11:44:40 wazuh-remoted: INFO: (1409): Authentication file changed. Updating.
2025/04/29 11:44:40 wazuh-remoted: INFO: (1410): Reading authentication keys file.

