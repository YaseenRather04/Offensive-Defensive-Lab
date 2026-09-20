Experiment 1: Scanning for Vulnerabilities in a Network
Objective
To identify active hosts, open ports, running services, and known vulnerabilities on a target network using Nmap and Nessus.
Tools Used
- Kali Linux
- Nmap
- Nessus
- Target machine
Procedure
1. Check IP Address and Connectivity
Check the IP address of the Kali machine and verify connectivity with the target machine.
ifconfig
ping <target-ip>
2. Discover Live Hosts
Scan the network to identify active hosts.
nmap -sn 192.168.56.0/24
3. Scan Open Ports
Scan the target machine to identify open ports and services.
nmap -sS <target-ip>
4. Detect Services and OS
Detect the service versions and operating system of the target machine.
nmap -sV -O <target-ip>
5. Save Nmap Results
Save the Nmap scan results into a text file.
nmap -sV -oN nmap_scan_results.txt <target-ip>
6. Launch Nessus
Open the Nessus web interface and select New Scan → Basic Network Scan.
https://localhost:8834
7. Configure and Launch the Scan
Enter the target IP address, save the configuration, and launch the scan.
8. Review Vulnerabilities
Review the vulnerabilities detected by Nessus according to their severity: Critical, High, Medium, Low, and Info.
9. Document Findings
Record the CVE ID, affected service/port, and recommended remediation for Critical and High vulnerabilities.
Result
The active hosts, open ports, running services, and known vulnerabilities of the target network were identified and documented.
Note
This experiment should be performed only on an authorized lab network or test machine.
