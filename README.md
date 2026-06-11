# Phishing Attack Simulation & Defense Lab

> **Ethical Notice:** This project was conducted in a fully isolated local environment. No real emails were sent to any real person. All techniques documented here are for authorized security testing and educational purposes only. Unauthorized phishing is illegal under the CAN-SPAM Act, the Computer Fraud and Abuse Act (CFAA), and equivalent laws worldwide.

---

## Overview

A hands-on cybersecurity lab simulating a full phishing campaign lifecycle — from infrastructure setup and email delivery to credential capture, analytics, and technical defense implementation. Built on Kali Linux using industry-standard open-source tools used by real penetration testers and SOC teams.

This project demonstrates practical knowledge of social engineering attack vectors, email authentication protocols, and phishing mitigation strategies relevant to real-world Blue Team and SOC Analyst roles.

---

## Skills Demonstrated

| Category | Skills |
|---|---|
| **Offensive (Ethical)** | Phishing campaign design, credential harvesting, SMTP configuration, landing page construction |
| **Defensive** | SPF / DKIM / DMARC analysis, MFA evaluation, email header forensics, phishing detection |
| **Tools** | Gophish, Mailhog, Kali Linux, dig (DNS lookup), Nano |
| **Soft Skills** | Security awareness training design, executive-style incident reporting |

---

## Tools & Environment

| Tool | Purpose |
|---|---|
| **Kali Linux** | Attack and defense platform |
| **Gophish** | Open-source phishing simulation framework |
| **Mailhog** | Local SMTP mail catcher (no real internet traffic) |
| **Firefox** | Victim browser simulation |
| **dig** | DNS record lookup for SPF / DMARC verification |

---

## Lab Architecture

```
┌─────────────────────────────────────────────────────┐
│                  LOCAL MACHINE ONLY                  │
│                                                     │
│  Gophish Admin Panel  →  https://localhost:3333     │
│  Gophish Phishing Server → http://localhost:80      │
│  Mailhog SMTP Catcher →  localhost:2025             │
│  Mailhog Web Inbox    →  http://localhost:8025      │
│                                                     │
│  ✅ Zero internet traffic. Fully self-contained.    │
└─────────────────────────────────────────────────────┘
```

---

## Project Breakdown

### Task 1 — Campaign Infrastructure Setup
Configured all four components required to run a professional phishing simulation:
- **Sending Profile** — Local Mailhog SMTP server (127.0.0.1:2025) configured as the mail relay
- **User Group** — Single self-targeted test recipient (authorized test only)
- **Email Template** — HTML phishing email exploiting urgency, authority, and fear triggers
- **Landing Page** — Fake credential capture form with post-submission redirect

**Key learning:** Real attackers invest heavily in the Sending Profile — registering lookalike domains (e.g. `paypa1.com`) with valid SPF/DKIM records to bypass spam filters.

---

### Task 2 — Campaign Execution
Launched the phishing campaign through Gophish and simulated the victim experience end-to-end:
1. Email delivered to Mailhog inbox
2. Phishing link clicked → redirected to fake landing page
3. Fake credentials submitted → captured by Gophish
4. Victim redirected to Google post-submission

---

### Task 3 — Campaign Analytics & Results Interpretation
Analyzed the Gophish campaign dashboard, reviewing the full event chain:

| Event | Description |
|---|---|
| **Email Sent** | Delivered to target inbox |
| **Email Opened** | Tracked via hidden 1×1 pixel beacon image |
| **Clicked Link** | Target interacted with phishing link |
| **Submitted Data** | Credentials captured on fake landing page |

**Industry context:** In untrained organizations, click rates of 10–15% and credential submission rates of 2–5% are typical. Any credential submission rate above 0% is a critical finding in a real engagement.

---

### Task 4 — Technical Defense Implementation
Researched and analyzed the primary technical controls against phishing:

**Email Authentication Stack:**
- **SPF (Sender Policy Framework)** — Verifies that the sending mail server is authorized by the domain owner; prevents domain spoofing
- **DKIM (DomainKeys Identified Mail)** — Cryptographic signature that proves the email was not tampered with in transit
- **DMARC** — Policy layer that combines SPF and DKIM; instructs receiving servers to reject or quarantine unauthenticated mail

Verified real-world implementation using DNS lookups:
```bash
dig TXT google.com | grep spf
dig TXT _dmarc.google.com
```

**MFA Analysis:**
Evaluated how Multi-Factor Authentication breaks the attack chain even after credentials are stolen. Hardware keys (FIDO2/WebAuthn) are phishing-resistant; SMS-based MFA can be defeated by real-time proxy attacks.

---

### Task 5 — Phishing Detection Checklist (User Awareness)
Developed an 8-point phishing detection checklist for non-technical users based on red flags observed during the simulation:

1. ✅ Does the sender email address exactly match the company domain?
2. ✅ Does the URL shown on hover match the expected domain?
3. ✅ Does the email use a generic greeting instead of your actual name?
4. ✅ Is there urgency or threat language pressuring immediate action?
5. ✅ Is the email asking you to verify credentials by clicking a link?
6. ✅ Did you initiate this interaction, or did it arrive unexpectedly?
7. ✅ Does the email contain spelling errors or unusual formatting?
8. ✅ Can you verify the request through a separate known channel (phone, direct website)?

**Most commonly missed red flag:** The sender display name — users see a familiar name and don't check the actual email address behind it.

---

### Task 6 — Professional Engagement Report
Produced a structured phishing simulation report following professional penetration testing conventions, including:
- Campaign methodology and configuration
- Full event chain results and metrics
- Attack chain analysis (psychological techniques used)
- Technical findings (SPF/DKIM/DMARC gaps, MFA impact)
- Prioritized remediation recommendations
- Business Email Compromise (BEC) research and comparison

---

## Key Takeaways

- Phishing attacks target human psychology, not software — making user awareness training as critical as technical controls
- SPF/DKIM/DMARC together prevent many phishing emails from reaching inboxes, but require correct configuration to be effective
- **MFA is the single highest-impact technical control** — it protects accounts even after credentials are stolen
- The "urgency + authority + fear" psychological triad is the core engine of effective phishing emails
- Real-time phishing proxy attacks (e.g. Evilginx2) can defeat certain MFA types, making hardware keys the gold standard

---

## References

- [Gophish Official Documentation](https://docs.getgophish.com/)
- [Mailhog Project](https://github.com/mailhog/MailHog)
- [Anti-Phishing Working Group (APWG)](https://apwg.org/)
- [Google Phishing Quiz](https://phishingquiz.withgoogle.com/)
- [VirusTotal — Suspicious URL Scanner](https://www.virustotal.com/)
- [DMARC Lookup Tool — MXToolbox](https://mxtoolbox.com/dmarc.aspx)

---

## Author

**David Perez Lozada**  
CompTIA Security+ (SY0-701) | CompTIA A+  
[davidfernandoperez349@gmail.com](mailto:davidfernandoperez349@gmail.com)
