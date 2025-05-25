# Wazuh Labs – Blue Team Practical

> รวม Lab การใช้งาน Wazuh สำหรับการเรียนรู้ด้าน Blue Team
> จัดทำโดย: [jakkrinSec](https://github.com/jakkrinSec)

## Introduction
โปรเจกต์นี้จัดทำขึ้นเพื่อทดสอบและฝึกฝนการใช้งาน **Wazuh** สำหรับวัตถุประสงค์ด้านความมั่นคงปลอดภัยไซเบอร์  
โดยเฉพาะในด้าน **การตรวจจับการบุกรุก (Intrusion Detection)** และ **การตอบสนองเหตุการณ์ (Incident Response)**

## System Setup
โปรเจกต์นี้ติดตั้งบน VirtualBox
- [Install Ubantu 22.04 บน Virtual Box](https://github.com/jakkrinsec/Wazuh_Labs/blob/main/README.md#1-%E0%B8%95%E0%B8%B4%E0%B8%94%E0%B8%95%E0%B8%B1%E0%B9%89%E0%B8%87-ubuntu-2204-%E0%B8%9A%E0%B8%99-virtualbox)
- Ubuntu 22.04 - Wazuh Server
- Ubantu 22.04 - Ubuntu Agent
- Windows11 - Windows Agent
  
## Lab Contents
| Lab | หัวข้อ | รายละเอียด |
|-----|--------|------------|
| 1   | [File Integrity Monitoring (FIM)](https://github.com/jakkrinsec/Wazuh_Labs/blob/main/README.md#lab-1--file-integrity-monitoring-fim) | ตรวจจับการเปลี่ยนแปลงไฟล์ใน Directory ที่กำหนดแบบ Real-time |
| 2   | [Monitor Docker Event](https://github.com/jakkrinsec/Wazuh_Labs/blob/main/README.md#lab-2--monitor-docker-event) | ตรวจจับการเปลี่ยนแปลงใน Docker ที่กำหนดแบบตาม Interval |
| 3   | ⏳ Soon... | กำลังจะเพิ่มเร็ว ๆ นี้ |

### 1. ติดตั้ง Ubuntu 22.04 บน VirtualBox
  - ดาวน์โหลด ISO:  
   [Ubuntu Server 22.04 LTS](https://ubuntu.com/download/server)
  - สร้าง VM และแนบ ISO เป็น Optical Drive
  ![image](https://github.com/user-attachments/assets/b35d13fb-d9a4-43eb-9e1d-17e9e338a182)
  - ติดตั้ง Ubuntu Server
  - หลังติดตั้งเสร็จ ล็อกอินและอัปเดตระบบ
  ``` bash
  sudo apt update && sudo apt upgrade -y
  ```
   
### 3. ติดตั้ง Wazuh Server (All-in-one)
  - ติดตั้งเครื่องมือที่จำเป็น
  ``` bash
  sudo apt install curl apt-transport-https unzip wget gnupg -y
  ```
  
  - ดาวน์โหลดและรัน Wazuh Installation Assistant
  ``` bash
  # ดาวน์โหลดสคริปต์ติดตั้ง
  curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh

  # ให้สิทธิ์ในการรันสคริปต์
  chmod +x wazuh-install.sh

  # รันสคริปต์ติดตั้งแบบ All-in-one (Wazuh Server + Indexer + Dashboard)
  sudo ./wazuh-install.sh -a
  ```
  
### 4. เข้าสู่ Wazuh Dashboard
  - เปิดเว็บบราวเซอร์ เข้าไปที่ https://[IP-ของ-Wazuh]
  - ยอมรับ (self-signed certificate)
  - ใช้ Username/Password ที่แสดงหลังติดตั้ง ตัวอย่าง
  ``` bash
  23/05/2025 09:00:01 INFO: --- Summary ---
  23/05/2025 09:00:01 INFO: You can access the web interface https://<wazuh-dashboard-ip>:443
    User: admin
    Password: D7S.WdFYGs1nUl240vcyYggQ6s?XHSTU
  23/05/2025 09:00:01 INFO: Installation finished.
  ```
  - User: admin
  - Password: D7S.WdFYGs1nUl240vcyYggQ6s?XHSTU

  ตัวอย่างหน้า Login
  ![image](https://github.com/user-attachments/assets/34bdbd6d-d204-4ee2-88a0-de6b6471eb22)

### 5. ติดตั้ง Windows 11
  - VirtualBox (ดาวน์โหลดได้ที่: https://www.virtualbox.org/)
  - Windows 11 ISO (ดาวน์โหลดจากเว็บ Microsoft: https://www.microsoft.com/software-download/windows11)
  - RAM อย่างน้อย 4 GB และพื้นที่ว่างในเครื่อง 30+ GB
  ![image](https://github.com/user-attachments/assets/a17b61ab-7d4d-4b3e-9178-6966a05e1a67)

### 6. ติดตั้ง Wazuh Agent บน Windows 11
 - ไปที่ Wazuh Dashboard > Agents > Deploy 
 - เลือกระบบปฏิบัติการ → Windows
 - ทำตามคำแนะนำการติดตั้ง
 - ตัวอย่างหน้าจอ:

   ![image](https://github.com/user-attachments/assets/48a52024-b667-41ab-bae4-3828096adac8)
   ![image](https://github.com/user-attachments/assets/86364b3b-ea6c-4bd8-8240-385b6c9f0421)

   ![image](https://github.com/user-attachments/assets/c5bcfa22-8cb5-4a61-a76b-6261fa7743ec)


### 7. ตรวจสอบการเชื่อมต่อ Windows11 - Agent
 - ไปที่เมนู Agents → ตรวจสอบว่า Agent แสดงสถานะ Active
   ![image](https://github.com/user-attachments/assets/8bcf3556-31e1-4064-9024-f7d496dbd70c)

 - หรือดู Log ที่ Wazuh Server:
  ``` bash
  jakkrinsec@WazuhServer:~$ sudo tail -f /var/ossec/logs/ossec.log
  ```
  - ตัวอย่าง Log:
  ``` bash
2025/05/23 15:50:25 sca: INFO: Security Configuration Assessment scan finished. Duration: 5 seconds.
2025/05/23 15:50:54 rootcheck: INFO: Ending rootcheck scan.
2025/05/23 16:50:21 wazuh-modulesd:syscollector: INFO: Starting evaluation.
2025/05/23 16:50:31 wazuh-modulesd:syscollector: INFO: Evaluation finished.
2025/05/23 17:42:50 wazuh-authd: INFO: New connection from 192.168.1.201
2025/05/23 17:42:50 wazuh-authd: INFO: Received request for a new agent (Windows11) from: 192.168.1.201
2025/05/23 17:42:50 wazuh-authd: INFO: Agent key generated for 'Windows11' (requested by any)
2025/05/23 17:42:57 wazuh-remoted: INFO: (1409): Authentication file changed. Updating.
2025/05/23 17:42:57 wazuh-remoted: INFO: (1410): Reading authentication keys file.
2025/05/23 17:43:12 wazuh-remoted: WARNING: Agent key already in use: agent ID '001'
  ```
### 8. ติดตั้ง Wazuh Agent บน Ubantu
  - ไปที่ Wazuh Dashboard > Agents > Deploy 
  - เลือกระบบปฏิบัติการ → Linux(DEB amd64)
  - ทำตามคำแนะนำการติดตั้ง
  - ตัวอย่างหน้าจอ:
    
    ![image](https://github.com/user-attachments/assets/1ca2bea0-7d1f-442e-bfc6-19dcd50babf6)
    ![image](https://github.com/user-attachments/assets/a8c102e2-0e56-4c1a-9321-0d9c52692bb7)

### 9. ตรวจสอบการเชื่อมต่อ Ubuntu - Agent
  - ไปที่เมนู Agents → ตรวจสอบว่า Agent แสดงสถานะ Active
  ![image](https://github.com/user-attachments/assets/da5ae101-ef6a-4b01-9391-abe3efc1aa43)
  - หรือดู Log ที่ Wazuh Server:
  ``` bash
  jakkrinsec@WazuhServer:~$ sudo tail -f /var/ossec/logs/ossec.log
  ```
  - ตัวอย่าง Log:
  ``` bash
2025/05/23 18:04:26 wazuh-modulesd:syscollector: INFO: Starting evaluation.
2025/05/23 18:04:26 wazuh-modulesd:syscollector: INFO: Evaluation finished.
2025/05/23 18:04:31 sca: INFO: Evaluation finished for policy '/var/ossec/ruleset/sca/cis_ubuntu22-04.yml'
2025/05/23 18:04:31 sca: INFO: Security Configuration Assessment scan finished. Duration: 5 seconds.
2025/05/23 18:05:05 rootcheck: INFO: Ending rootcheck scan.
2025/05/23 18:27:37 wazuh-authd: INFO: New connection from 192.168.1.202
2025/05/23 18:27:37 wazuh-authd: INFO: Received request for a new agent (Ubuntu) from: 192.168.1.202
2025/05/23 18:27:37 wazuh-authd: INFO: Agent key generated for 'Ubuntu' (requested by any)
2025/05/23 18:27:46 wazuh-remoted: INFO: (1409): Authentication file changed. Updating.
2025/05/23 18:27:46 wazuh-remoted: INFO: (1410): Reading authentication keys file.
  ```
  ---

## Lab Contents
| Lab | หัวข้อ | รายละเอียด |
|-----|--------|------------|
| 1   | [File Integrity Monitoring (FIM)](https://github.com/jakkrinsec/Wazuh_Labs/blob/main/README.md#lab-1--file-integrity-monitoring-fim) | ตรวจจับการเปลี่ยนแปลงไฟล์ใน Directory ที่กำหนดแบบ Real-time |
| 2   | [Monitor Docker Event](https://github.com/jakkrinsec/Wazuh_Labs/blob/main/README.md#lab-2--monitor-docker-event) | ตรวจจับการเปลี่ยนแปลงใน Docker ที่กำหนดแบบตาม Interval |
| 3   | ⏳ Soon... | กำลังจะเพิ่มเร็ว ๆ นี้ |


## Lab 1 – File Integrity Monitoring (FIM)
### Objective
ทดสอบการตรวจสอบความสมบูรณ์ของไฟล์ (File Integrity Monitoring) โดยใช้ Wazuh Agent เพื่อตรวจจับการ **สร้าง**, **แก้ไข**, และ **ลบ** ไฟล์ในไดเรกทอรีที่กำหนด
### Configuration
### 1. Windows11 Endpoint
#### 1.1 กำหนด Directory ที่ต้องการ Monitor
  - แก้ไขไฟล์ `ossec.conf` บนเครื่อง Agent เพื่อกำหนด Directory
     - ไปที่ C:\Program Files (x86)\ossec-agent\ossec.conf
  - ไปที่ section `<syscheck>` และเพิ่ม directory ที่ต้องการตรวจสอบแบบ Real-time
    - ในตัวอย่างเลือก Monitor ที่ C:\Users\Jakkrinsec\DEMO Folder
  ``` bash
  <directories check_all="yes" report_changes="yes" realtime="yes">C:\Users\jakkr\Desktop</directories>
  ```
#### 1.2 Restart Wazuh Agent 
  - เปิด PowerShell ด้วยสิทธิ์ Administrator และรันคำสั่ง:
  ``` bash
  Restart-Service -Name wazuh
  ```
### 2. Ubuntu Endpoint
#### 2.1 กำหนด Directory ที่ต้องการ Monitor
  - แก้ไขไฟล์ `ossec.conf` บนเครื่อง Agent เพื่อกำหนด Directory
     - ไปที่ /var/ossec/etc/ossec.conf
  - ไปที่ section `<syscheck>` และเพิ่ม directory ที่ต้องการตรวจสอบแบบ Real-time
    - ในตัวอย่างเลือก Monitor ที่ C/home/jakkrinsec/DEMOFolder
  ``` bash
  <directories check_all="yes" report_changes="yes" realtime="yes">/home/jakkrinsec/DEMOFolder</directories>
  ```
#### 2.2 Restart Wazuh Agent เพื่อ Apply
  - รันคำสั่ง:
  ``` bash
  sudo systemctl restart wazuh-agent
  ```
### 3 ทดสอบสร้างไฟล์ แก้ไข และลบ
  - Create/edit/delete .txt file
### 4 ตรวจสอบ Event บน Dashboard
  - เข้าสู่ Wazuh Dashboard
  - ไปที่เมนู Integrity Monitoring → Events
  - ตรวจสอบเหตุการณ์ที่มี rule.id ดังนี้:
    - 550 - New file created
    - 553 - File modified
    - 554 - File deleted
  - ตัวอย่าง Event: Windows11
    ![image](https://github.com/user-attachments/assets/c1355698-a000-4c9b-99e9-1eff4c4cfa9a)
  - ตัวอย่าง Event: Ubantu  
    ![image](https://github.com/user-attachments/assets/fac4e647-10b7-423c-9bb8-0bf38ed776d1)
### Summary
- ตรวจจับการเปลี่ยนแปลงไฟล์ใน Directory ที่กำหนดแบบ Realtime
- ตรวจสอบ Event ได้จาก Wazuh Dashboard ด้วย Rule ID (550: New file Create, 553: File modified และ 554: File Delete)


## Lab 2 – Monitor Docker Event
### Objective
ทดสอบตรวจจับกิจกรรม เช่น การสร้าง ลบ หรือเปลี่ยนแปลงสถานะของคอนเทนเนอร์ และส่งข้อมูลไปยัง Wazuh server เพื่อวิเคราะห์และแสดงผลบนแดชบอร์ด โดยใช้โมดูล Docker-listener

### Configuration
#### 1. ติดตั้ง Docker and Python Docker Library
  ``` bash
  sudo apt install python3 python3-pip #ติดตั้ง Python and pip
  pip3 install --upgrade pip #upgrade pip
  curl -sSL https://get.docker.com/ | sh
  sudo pip3 install docker==7.1.0 urllib3==1.26.20 requests==2.32.2
  ```
#### 2. Enable Docker-listener
  - แก้ไขไฟล์ /var/ossec/etc/ossec.conf โดยเพิ่มคำสั่ง:
  ``` bash
  <ossec_config>
    <wodle name="docker-listener">
      <interval>10m</interval>
      <attempts>5</attempts>
      <run_on_start>yes</run_on_start>
      <disabled>no</disabled>
    </wodle>
  </ossec_config>
  ```
#### 3 Restart Wazuh Agent เพื่อ Apply
  - รันคำสั่ง:
  ``` bash
  sudo systemctl restart wazuh-agent
  ```
#### 4 Pull an image, such as the NGINX image, and run a container
``` bash
sudo docker pull nginx #ดาวน์โหลด nginx image เวอร์ชันล่าสุดจาก Docker Hub
sudo docker run -d -P --name nginx_container nginx #สร้างและรัน container จาก image ตั้งชื่อให้ container นี้ว่า nginx_container ใช้ image ชื่อ nginx ที่เราดาวน์โหลดมาก่อนหน้านี้
sudo docker exec -it nginx_container cat /etc/passwd #แสดงรายชื่อ users ที่อยู่ใน container 
sudo docker exec -it nginx_container /bin/bash #เข้าสู่ shell ภายใน container
exit
sudo docker port nginx_container #ตรวจสอบ Port เพื่อ Login
```
#### 5 เข้า nginx ผ่าน Browser
  - รันคำสั่ง:
  ``` bash
  sudo docker port nginx_container #ตรวจสอบ Port เพื่อ Login
  80/tcp -> 0.0.0.0:32768 #ตัวอย่างการ Reply
  ```
  - ตัวอย่างการเข้า nginx ผ่าน Browser
  ![image](https://github.com/user-attachments/assets/21c27374-fd5d-49fa-b858-227ba706df82) 
#### 6 ทดสอบแก้ไขหน้า Index
  - รันคำสั่ง:
  ``` bash
  sudo docker exec -it nginx_container bash #ล็อคอินเข้า shell
  cd /usr/share/nginx/html #ไปที่ directory html
  echo "Hello from jakkrinsec" > index.html
  ```
  - ตัวอย่างการแก้ไขหน้า Index
  ![image](https://github.com/user-attachments/assets/18c2f55d-59c0-4194-a522-dce66a8db8a0)
#### 7 ตรวจสอบ Event บน Dashboard
  - เข้าสู่ Wazuh Dashboard
  - ไปที่เมนู Integrity Monitoring → Events (Filter rule.group:Docker)
  - ตรวจสอบเหตุการณ์ที่มี rule.id ดังนี้:
    - 87907 - Command launched in container nginx_container
    - 87924 - Container nginx_container received the action
  ![image](https://github.com/user-attachments/assets/1e25fe10-4007-4016-9361-a08a730c6164)
### Summary
- ตรวจจับการเปลี่ยนแปลงใน Docker ที่กำหนดแบบตาม Interval
