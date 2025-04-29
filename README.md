# Wazuh
รองรับการติดตั้งบน
Red Hat Enterprise Linux 7, 8, 9; 
CentOS 7, 8; 
Amazon Linux 2; 
Ubuntu 16.04, 18.04, 20.04, 22.04.
โดยเราจะเลือก ติดตั้งบน Ubantu 22.04

ขั้นตอนที่ 1: เตรียมระบบ Ubuntu
หลังติดตั้งเสร็จ ล็อคอินและอัพเดต
sudo apt update && sudo apt upgrade -y
sudo apt install curl apt-transport-https unzip wget gnupg openssh-server -y

ขั้นตอนที่ 2: ดาวน์โหลดและรัน Wazuh Installation Assistant
# ดาวน์โหลดสคริปต์ติดตั้ง
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
# ให้สิทธิ์ในการรันสคริปต์
chmod +x wazuh-install.sh
# รันสคริปต์ติดตั้งแบบ All-in-one (Wazuh Server + Indexer + Dashboard)
sudo ./wazuh-install.sh -a


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

