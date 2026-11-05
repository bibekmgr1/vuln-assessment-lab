# Vulnerability Assessment Lab

## What this is
I built a lab in Docker to practice scanning and verifying vulnerabilities. I didn't want to just run a scanner and take the results as absolute truth, so I used Nmap and Wireshark to check the findings myself.

**Tools:** Nessus Essentials, Nmap, Wireshark
**Target:** Local Docker Container (172.17.0.2)

## What I did (in order of the screenshots)

### 1. Basic Scan Setup
First, I set up a basic network scan in Nessus and put in the target IP. 

### 2. Adding SSH Credentials
Then, I added SSH credentials to the scan. This lets Nessus log into the machine, so it can find internal config issues instead of just looking at open ports from the outside.

### 3. Looking at the Results
The scan finished and found 19 vulnerabilities. I sorted them by severity to see what I needed to look at first.

### 4. Samba Badlock (CVE-2016-2118)
The biggest finding was Samba Badlock. Nessus gave it a High severity and a CVSS score of 7.5. It's an issue with how Samba handles authentication, which can let an attacker do man-in-the-middle attacks. Nessus recommended updating to Samba 4.2.11 or later.

### 5. Nmap Verification
I didn't want to just trust the scanner, so I ran Nmap myself to check the Samba version. I used the `smb-os-discovery` script. The output showed "Samba 3.0.20-Debian", which means it's definitely vulnerable.

### 6. Wireshark (FTP)
I also had Wireshark running to look at FTP traffic. I filtered for FTP and saw the `USER` and `PASS` commands go across the screen. Because FTP is unencrypted, I could read the password in plain text. I forgot to take a screenshot of this, but seeing the plain text credentials go across the screen helped me understand why unencrypted protocols are a security risk.
