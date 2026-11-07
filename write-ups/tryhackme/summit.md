# TryHackMe: Summit — Room Review & Defensive Concepts

> **Note:** "Summit" is a subscriber-only room on TryHackMe. To respect the platform's Terms of Service and protect the learning experience of others, this repository contains redacted flags, answers, or direct solutions. Instead, it documents my understanding of the defensive concepts and tools utilized during the challenge.

## Scenario & Objective
In this challenge, I acted as a Detection Engineer at **PicoSecure**, engaging in a live back-and-forth simulation against a threat actor named **Sphinx**. The primary objective was to successfully detect and mitigate an adapting adversary by escalating defenses through the **Pyramid of Pain** framework.

## Core Concepts Practiced

### 1. The Pyramid of Pain Implementation
The core mechanic of this room requires shifting from reactive, low-level indicators to proactive, high-level behavioral detection:
*   **Bottom of the Pyramid (Hashes & IPs):** Blocked initial samples via simple indicator matching (YARA / Firewall rule definitions). Observed how quickly the adversary bypassed these trivial blocks.
*   **Middle of the Pyramid (Domains & Tools):** Advanced to blocking communication infrastructure and identifying malicious software artifacts.
*   **Top of the Pyramid (TTPs / Tactics, Techniques, & Procedures):** Implemented high-fidelity detection rules targeting attacker behavior, specifically focusing on unauthorized discovery techniques and registry/file system anomalies.

### 2. Detection Engineering & Rule Building
*   **Sigma Rules:** Built logic to alert on specific host and network behaviors using event data.
*   **Sysmon Logs:** Analyzed process creation, network connections, and file creation events (`File Creation and Modification` tracking) to identify malicious patterns.
*   **MITRE ATT&CK Mapping:** Aligned specific malicious actions (like attacker data staging or host profiling) to official tactics, specifically **Discovery (TA0007)**.

## Tools Highlighted
*   **Sigma Rule Builder** (Simulated rule generation for endpoint telemetry)
*   **Firewall & DNS Controls** (Network-level containment logs)
*   **Sysmon Event Analysis** (Host-level behavioral auditing)

## Key Takeaways
This lab perfectly illustrated why relying on simple IOCs (Indicators of Compromise) like MD5 hashes is ineffective for modern defense. By forcing the adversary to change their entire attack methodology (TTPs), we dramatically increase the "cost of entry" for the attacker, eventually forcing them to back down.

## Walkthrough

#### sample1.exe 

This is the first sample in this room. 
The goal is to find IoCs to improve the defenses.

I decided to first perform a static analysis on this executable as it is the most common first step in the detection phase. 
Looking at the hashes, the executable is tagged as 'Trojan.Metasploit.A'. This is already a red flag since Metasploit is a known open-source framework that is used to find, exploit, and validate system vulnerabilities, and is commonly used by attackers/script kiddies. 
> Side note: Hashes are static IoCs. A match can be an excellent indicator, but an additional operational check should be done as well. Also, check the detection ratio to better gauge the reliability of the verdict.


