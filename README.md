# Wazuh Labs – Blue Team Practical

> รวม Lab การใช้งาน Wazuh สำหรับการเรียนรู้ด้าน Blue Team / SOC Operations  
> จัดทำโดย: [jakkrinSec](https://github.com/jakkrinSec)

## Introduction
โปรเจกต์นี้จัดทำขึ้นเพื่อทดสอบและฝึกฝนการใช้งาน **Wazuh** สำหรับวัตถุประสงค์ด้านความมั่นคงปลอดภัยไซเบอร์  
โดยเฉพาะในด้าน **การตรวจจับการบุกรุก (Intrusion Detection)** และ **การตอบสนองเหตุการณ์ (Incident Response)**

## Before You Start – System Setup
### 1. Operating System Requirements
  - Wazuh รองรับ OS:
    - Red Hat Enterprise Linux 7, 8, 9
    - CentOS 7, 8
    - Amazon Linux 2
    - Ubuntu 16.04, 18.04, 20.04, **22.04 (แนะนำ)**
  > ในโปรเจกต์นี้เลือกใช้ **Ubuntu 22.04 Server** ที่ติดตั้งบน VirtualBox

### 2. ติดตั้ง Ubuntu 22.04 บน VirtualBox
  - ดาวน์โหลด ISO:  
   [Ubuntu Server 22.04 LTS](https://ubuntu.com/download/server)
  - สร้าง VM และแนบ ISO เป็น Optical Drive
  - ติดตั้ง Ubuntu Server (เลือก OpenSSH Server)
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
  29/04/2025 11:15:55 INFO: --- Summary ---
  29/04/2025 11:15:55 INFO: You can access the web interface https://<wazuh-dashboard-ip>:443
      User: admin
      Password: 5ED5fSka*2nE5?xw6?gE7Yuw8NT2H.il
  29/04/2025 11:15:55 INFO: Installation finished
  ```
  > User: admin
  > Password: 5ED5fSka*2nE5?xw6?gE7Yuw8NT2H.il

  ตัวอย่างหน้า Login
  ![image](https://github.com/user-attachments/assets/34bdbd6d-d204-4ee2-88a0-de6b6471eb22)

### 5. ติดตั้ง Wazuh Agent บน Windows 10
 - ไปที่ Wazuh Dashboard > Agents > Deploy 
 - เลือกระบบปฏิบัติการ → ทำตามคำแนะนำการติดตั้ง
 - ตัวอย่างหน้าจอ:
  ![image](https://github.com/user-attachments/assets/41bc29c4-ecb8-4102-bdfb-612eaa1a25e1)
  ![image](https://github.com/user-attachments/assets/5e0fd02f-9114-4b1c-b0b9-c989c5b77169)

### 6. ตรวจสอบการเชื่อมต่อ Agent
 - ไปที่เมนู Agents → ตรวจสอบว่า Agent แสดงสถานะ Active
   ![image](https://github.com/user-attachments/assets/8ef04b00-63cf-4746-a6b1-e6a409cbe446) 
 - หรือดู Log ที่ Wazuh Server:
  ``` bash
  jakkrinsec@WazuhServer:~$ sudo tail -f /var/ossec/logs/ossec.log
  ```
  - ตัวอย่าง Log:
  ``` bash
  [sudo] password for jakkrinsec:
  2025/04/29 11:44:36 wazuh-authd: INFO: New connection from 192.168.1.101
  2025/04/29 11:44:36 wazuh-authd: INFO: Received request for a new agent (My_PC) from: 192.168.1.101
  2025/04/29 11:44:36 wazuh-authd: INFO: Agent key generated for 'My_PC' (requested by any)
  2025/04/29 11:44:40 wazuh-remoted: INFO: (1409): Authentication file changed. Updating.
  2025/04/29 11:44:40 wazuh-remoted: INFO: (1410): Reading authentication keys file.
  ```

  ---

## Lab Contents
| Lab | หัวข้อ | รายละเอียด |
|-----|--------|------------|
| 1   | File Integrity Monitoring (FIM) | ตรวจจับการเปลี่ยนแปลงไฟล์แบบ Real-time |
| 2   | ⏳ Soon... | กำลังจะเพิ่มเร็ว ๆ นี้ |


## Lab 1 – File Integrity Monitoring (FIM)
### Objective
ทดสอบการตรวจสอบความสมบูรณ์ของไฟล์ (File Integrity Monitoring) โดยใช้ Wazuh Agent เพื่อตรวจจับการ **สร้าง**, **แก้ไข**, และ **ลบ** ไฟล์ในไดเรกทอรีที่กำหนด

### Setup Instructions
#### 1. กำหนด Directory ที่ต้องการ Monitor
  - แก้ไขไฟล์ `ossec.conf` บนเครื่อง Agent:
     - C:\Program Files (x86)\ossec-agent\ossec.conf
  - ไปที่ section `<syscheck>` และเพิ่ม directory ที่ต้องการตรวจสอบแบบ Real-time:
  ``` bash
  <directories check_all="yes" report_changes="yes" realtime="yes">C:\Users\jakkr\Desktop</directories>
  ```
#### 2. Restart Wazuh Agent 
  - เปิด PowerShell ด้วยสิทธิ์ Administrator และรันคำสั่ง:
  ``` bash
  Restart-Service -Name wazuh
  ```
#### 3. ตรวจสอบสถานะ Agent
  - ตรวจสอบ Log บน Wazuh Server ด้วยคำสั่ง:
  ``` bash
  sudo tail -f /var/ossec/logs/ossec.log
  ```
  - ตัวอย่าง Log ที่เกี่ยวข้องกับ FIM:
  ``` bash
  2025/04/29 20:32:10 rootcheck: INFO: Ending rootcheck scan.
  2025/04/29 20:32:20 wazuh-agent: INFO: (6009): File integrity monitoring scan ended.
  2025/04/29 20:32:20 wazuh-agent: INFO: FIM sync module started.
  2025/04/29 20:32:22 wazuh-agent: INFO: (6012): Real-time file integrity monitoring started.
  ```
#### 4. ตรวจสอบ Event บน Dashboard
  - เข้าสู่ Wazuh Dashboard
  - ไปที่เมนู Integrity Monitoring → Events
  - ตรวจสอบเหตุการณ์ที่มี rule.id ดังนี้:
    - 550 - New file created
    - 553 - File modified
    - 554 - File deleted
  - ตัวอย่าง Event:
  ![image](https://github.com/user-attachments/assets/10263d37-613b-4abb-8360-cb50f2e2ca9d)

### Summary
- ติดตั้งและตั้งค่า FIM ได้สำเร็จ
- ตรวจจับการเปลี่ยนแปลงไฟล์ใน Directory ที่กำหนดแบบ Realtime
- ตรวจสอบ Event ได้จาก Wazuh Dashboard ด้วย Rule ID ที่เกี่ยวข้อง
