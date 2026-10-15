# Experiment 5 — Sniffing and Session Hijacking

> **Lab scope:** This exercise was performed on an isolated virtual lab network using Kali Linux and Metasploitable 2 with DVWA. Do not run ARP redirection or capture traffic on networks or devices without permission.

## Objective

Observe how an unencrypted HTTP login can be inspected when a machine on the same lab network is positioned between the client and its gateway. The exercise demonstrates ARP spoofing, packet capture, and the session security risk of HTTP.

## Lab setup

- **Analysis machine:** Kali Linux
- **Target:** Metasploitable 2 running DVWA
- **Target address shown in the evidence:** `192.168.1.100`
- **Gateway address shown in the evidence:** `192.168.1.1`
- **Capture interface:** `eth0`

The Kali screenshot shows `eth0` on the `192.168.1.0/24` network and a second interface on a separate network. The lab addresses above are specific to this run.

## Procedure and observations

1. **Confirm the lab hosts are reachable.** Kali received replies from both the target and gateway, with no packet loss in the displayed checks.

   ![Kali connectivity checks to the target and gateway](Screenshot%202026-10-15%20122202.png)

2. **Allow Kali to forward packets** so traffic can continue through the machine during the man-in-the-middle demonstration.

   ![IPv4 forwarding enabled on Kali](Screenshot%202026-10-15%20122611.png)

3. **Run ARP spoofing in both directions** between the target and gateway. The terminal captures show repeated ARP replies for each peer.

   ![ARP replies sent toward the target](Screenshot%202026-10-15%20122835.png)

   ![ARP replies sent toward the gateway](Screenshot%202026-10-15%20122925.png)

4. **Verify forwarding and inspect traffic.** The forwarding setting is shown as enabled. The capture also shows ICMP traffic and redirects while the target communicates with the gateway.

   ![IPv4 forwarding confirmation](Screenshot%202026-10-15%20123051.png)

   ![Wireshark view of traffic involving the target](Screenshot%202026-10-15%20123249.png)

5. **Generate a request from the target and capture DVWA HTTP traffic.** The target's connectivity check succeeds, and Wireshark shows a DVWA login POST over HTTP.

   ![Target connectivity check and HTTP request](Screenshot%202026-10-15%20123257.png)

   ![Wireshark showing a DVWA login POST in the lab capture](Screenshot%202026-10-15%20123526.png)

6. **Confirm the authenticated DVWA session.** The browser screenshot shows the DVWA home page with the account logged in.

   ![Authenticated DVWA page in the lab browser](Screenshot%202026-10-15%20124548.png)

## Result

The lab demonstrated that ARP spoofing can redirect local network traffic through Kali when forwarding is enabled. Wireshark observed the DVWA login exchange over HTTP, and the browser showed an authenticated session. Because HTTP does not encrypt these contents, credentials and session data can be exposed to a network observer. HTTPS and secure cookie settings help prevent this class of exposure.

## Evidence included

The `screenshots/` directory contains the nine figures embedded above. Four other submitted screenshots expose plaintext login fields or a session cookie value, so they are intentionally excluded from this GitHub-ready copy. Keep those originals private; if your instructor specifically requires them, provide them through a private submission channel or make properly redacted copies first.

## Reproduction note

These screenshots are from one virtual lab run; IP addresses and interface names may differ in another setup. Use only disposable lab accounts and isolated machines.

