# Detecting an SSH Brute-Force Attack with Wazuh
**Author:** Alvin Espinoza
**Date:** 2026-07-17
**Focus Area:** SOC Detection

---

## 1. Objective

The main goal of this lab was to simulate an SSH brute-force attack. I used a Kali VM and an Ubuntu server. They were both on the same network. I tried to simulate an attack that SOC analysts run into daily. Attackers attempting too many passwords foreshadows the first steps of a breach if it's not detected.

## 2. Environment & Tools

| Role              | Machine                    | IP Address   |
| ----------------- | -------------------------- | ------------ |
| Attacker          | Kali Linux                 | 192.168.64.7 |
| Defender / Target | Ubuntu Server + Wazuh SIEM | 192.168.64.5 |

**Tools used:**
- **Hydra** - SSH brute-force attack tool
- **Wazuh** - open-source SIEM for detection and alerting
- **sed / grep** - used to build and verify a custom password wordlist

**Target account:** `astorm` (local user on the Ubuntu host)

**Wordlist:** custom `passlist.txt`, built from `rockyou.txt` with a known test
password inserted and verified before the attack

## 3. Execution Log

1. Built `passlist.txt` from `rockyou.txt` and confirmed SSH was live on
   port 22 — the attack needs a login service to target.
2. Launched the brute-force from Kali:
   `hydra -l astorm -P passlist.txt ssh://192.168.64.5 -t 4 -V`
3. Hydra flooded the host with failed logins. SSH began dropping connections,
   which is expected behavior against a rapid attack.
4. Switched to Wazuh dashboard to see what the SIEM detected.

## 4. Results, Debrief, and Takeaways

My Wazuh dashboard revealed 128 authentication failures and 0 critical alerts. The
attack was caught but no actual compromise occurred. Rule ID 5763 fired at Level
10 severity.  Wazuh mapped the attack to MITRE ATT&CK T1110 and flagged compliance implications across PCI DSS, GDPR, HIPAA, and NIST-800-53.

Becoming an attacker and defender at the same time felt like playing good cop
and bad cop with myself. Building the wordlist, launching Hydra, and then
switching to Wazuh to watch it light up gave me a full picture of what
both sides see. It showed me the value of a SIEM. Besides detecting the threat, you also have to classify it and map it correctly. Also documentation is so important. I made the mistake of doing 70 labs without documenting them. No one knew.

## Screenshots

**1. Wazuh dashboard — 128 auth failures + MITRE chart**
![Wazuh dashboard showing 128 authentication failures](images/lab-01/01-dashboard.png)

**2. Alert detail — attacker IP, username, raw log**
![Document details showing source IP and raw sshd log](images/lab-01/02-alert-detail.png)

**3. Hydra attack running from Kali**
![Hydra brute force attack attempts scrolling on Kali](images/lab-01/03-hydra-attack.png)

**4. Events filtered by Rule 5763**
![Events view filtered to rule 5763 brute force](images/lab-01/04-rule-5763-events.png)

**5. Rule 5763 definition — MITRE + compliance frameworks**
![Rule 5763 showing MITRE and compliance mappings](images/lab-01/05-rule-definition.png)


---
*End of Report*


 

