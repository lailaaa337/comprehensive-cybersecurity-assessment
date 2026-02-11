# Password Auditing

### 1. Generate Wordlists (Crunch)
# 1-4 chars length
crunch 1 4 -o wordlist.txt
# 1-4 chars using only 'abcd'
crunch 1 4 abcd -o wordlist.txt
# 7 chars with specific pattern
crunch 7 7 -t cat@,%^ -o wordlist.txt

### 2. Online Brute Force (Hydra)
# SSH Login
hydra -l root -P wordlist.txt 192.160.70.22 ssh
# FTP Login on custom port
hydra -l root -P wordlist.txt -s 8020 192.160.70.22 ftp

### 3. Offline Hash Cracking
hashcat -a 0 -m 1800 hash.txt wordlist.txt