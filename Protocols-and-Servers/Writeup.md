# Room Write-up: Protocols and Servers (1 & 2)
**Platform:** TryHackMe | **Module:** Network Security

## 🎯 Room Objectives
These rooms cover the fundamentals of common network protocols (HTTP, FTP, POP3, SMTP, and IMAP), their inherent security vulnerabilities, and how to execute password attacks against them[cite: 22]. It also explores mitigation strategies using secure protocols like SSH and SSL/TLS[cite: 22].

---

## 🔓 Phase 1: Cleartext Protocol Interaction (POP3)
Many legacy protocols transmit data in cleartext, making them vulnerable to interception. To demonstrate this, I connected directly to a POP3 email server running on port 110 using Telnet[cite: 21]. 

Once connected, I manually authenticated using raw POP3 commands, logging in as the user `frank` with the password `D2xc9CgD`, and checked the mailbox status using the `STAT` command[cite: 21].

```bash
telnet 10.112.155.206 110
USER frank
PASS D2xc9CgD
STAT
```
## 💥 Phase 2: Password Brute-Forcing (IMAP)
When services do not implement rate limiting or account lockouts, they are vulnerable to brute-force attacks. I used hydra to launch a dictionary attack against an IMAP server at 10.112.158.237 targeting the user lazie.   By utilizing the rockyou.txt wordlist, Hydra successfully tested thousands of passwords and cracked the account, revealing the valid password butterfly
```Bash

hydra -l lazie -P rockyou.txt -f 10.112.158.237 imap -V
```
## 🔒 Phase 3: Secure Remote Access (SSH)
    To mitigate the risks of cleartext credentials being intercepted over the network, secure protocols like SSH (Secure Shell) must be used. I connected to the target machine (10.112.158.237) as the user mark via SSH.   This provided an encrypted terminal session on the Ubuntu 20.04.6 LTS server, allowing safe remote management and file access
```Bash

ssh mark@10.112.158.237
```
🎉 Key Takeaways

    Legacy protocols like POP3 (Port 110) and unencrypted IMAP transmit credentials in plain text and can be interacted with directly using tools like Telnet.

    Weak passwords on public-facing login portals can be easily compromised using automated brute-force tools like Hydra.

    Always use encrypted protocols (like SSH instead of Telnet, or IMAPS/POP3S) to protect credentials and traffic from network sniffing.
