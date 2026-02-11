# Defensive Measures (UFW)

### 1. Basic Setup
sudo apt-get install ufw
sudo ufw enable
sudo ufw status

### 2. Rules
# Block specific attacker
sudo ufw delete allow from 192.160.70.5
# Manage services
sudo ufw allow OpenSSH
sudo ufw disable