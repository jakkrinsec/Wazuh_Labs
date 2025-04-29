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
