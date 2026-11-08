# TryHackMe: Eviction — Room Review & Defensive Concepts

> **Note:** "Eviction" is a subscriber-only room on TryHackMe. To respect the platform's Terms of Service and protect the learning experience of others, this repository contains zero flags, answers, or direct solutions. Instead, it documents my understanding of the defensive concepts and tools utilized during the challenge.

## Scenario & Objective
In this challenge, I acted as "Sunny", a SOC Analyst at **E-corp**, who receives a classified intelligence about **APT28**, an APT Group attacking industries similar to E-corp (rare earth minerals manufacturer). The primary objective was to gather tactics, techniques, and procedures used by that APT group through the **MITRE ATT&CK Navigator** framework.

## Core Concepts Practiced
MITRE ATT&CK Navigator

### 1. The MITRE ATT&CK NAVIGATOR
The core mechanic of this room is to look for the presence of TTPs in the environment using MITRE ATT&CK framework:
* Tactics -
* Techniques -
* Procedure -

### 2. Detection Engineering & Rule Building
*   **MITRE ATT&CK Mapping:** Aligned specific malicious actions (like attacker data staging or host profiling) to official tactics. 

## Tools Highlighted
*   **MITRE ATT&CK NAVIGATOR** - a web-based, open-source tool designed to help cybersecurity professionals visualize, annotate, and explore the MITRE ATT&CK framework of adversary tactics, techniques, and procedures (TTPs)

## 💡 Key Takeaways
This lab illustrates how threat intelligence can help us better understand an attack and map them out to a specific group that we can use to scope our own organization for potential attacks and put controls in place to reduce the risk of an attack. 

## Walkthrough

### Question 1: What is a technique used by the APT to both perform recon and gain initial access?
Looking at the ATT&CK Navigator, particularly Reconnaissance and Initial Access, we can one common technique:

<img width="987" height="769" alt="image" src="https://github.com/user-attachments/assets/f3695b5a-feb7-4812-8e9b-b7751de74f33" />

**Spearphishing is a form of phishing attack that is more targeted to specific individuals or group.**

**🚩 Answer Clue:** <span style="font-size: 1.50em;">**`S****p******* L***`**</span>

### Question 2: Sunny identified that the APT might have moved forward from the recon phase. Which accounts might the APT compromise while developing resources?
Focusing on 'Resource Development', we can assume that the possible target of this APT group are email accounts. 

<img width="378" height="743" alt="image" src="https://github.com/user-attachments/assets/edb8cdf5-d3fc-4ca9-af1e-cfca7df13957" />

Keeping in mind that this group used spearphishing as a recon and initial access, spearphishing links are usually delivered through emails relying on account owners to unknowingly access the link to start the compromise.
This makes the answer a better fit than the other highlighted techniques. 

**🚩 Answer Clue:** <span style="font-size: 1.50em;">**`E**** A*******`**</span>

### Question 3: E-corp has found that the APT might have gained initial access using social engineering to make the user execute code for the threat actor. Sunny wants to identify if the APT was also successful in execution. What two techniques of user execution should Sunny look out for? (Answer format: <technique 1> and <technique 2>)

Let's first define 'User Execution'. User Execution is when an application or executable is manually triggered and ran. This action relies on user interaction to execute its payload. 

Going back to the MITRE ATT&CK Navigator, I can only see two highlighted techniques so this could be the answer: 
<img width="229" height="722" alt="image" src="https://github.com/user-attachments/assets/9b4742eb-0b00-4b91-b85f-407a9a25c9a5" />

This means that Sunny should look out possible malware (links, URLs, files, etc.) in their environment

**🚩 Answer Clue:** <span style="font-size: 1.50em;">**`M******** F*** AND M******** L***`**</span>

### Question 4: If the above technique was successful, which scripting interpreters should Sunny search for to identify successful execution? (Answer format: <technique 1> and <technique 2>)

Still under Execution, we aim to know which Command and Scripting Interpreter was used by an attacker to identify that an attack was successfully executed. Command and scripting interpreters are popular to attackers because these tools are being used legitimately and using it for attack can be hard to spot for defenders. This attack is called Living-Off-The-Land. It's a cybersecurity technique where adversaries use legitimate, pre-installed operating system tools, binaries, and scripts (such as PowerShell, WMI, or CertUtil) to execute malicious actions instead of dropping traditional malware files onto a target's disk.

<img width="257" height="297" alt="image" src="https://github.com/user-attachments/assets/0a043864-28e8-41e3-8b3f-1a389a887194" />

**🚩 Answer Clue:** <span style="font-size: 1.50em;">**`P********* AND W****** C****** S****`**</span>

### Question 5: While looking at the scripting interpreters identified in Q4, Sunny found some obfuscated scripts that changed the registry. Assuming these changes are for maintaining persistence, which registry keys should Sunny observe to track these changes?

This question is pretty straightforward. Attackers love to edit registry keys so they either run on startup and in the background. This creates a stealthy persistence against the user and the defenders almost like hiding in plain sight. 

**🚩 Answer Clue:** <span style="font-size: 1.50em;">**`R******* R** K***`**</span>

### Question 6: Sunny identified that the APT executes system binaries to evade defences. Which system binary's execution should Sunny scrutinize for proxy execution?

<img width="465" height="325" alt="image" src="https://github.com/user-attachments/assets/e3bfeb87-3c14-4bed-812d-388510a8b3ec" />

RunDll is a native, legitimate, core Windows system utility designed to load and run code stored in DLL. 

Attackers frequently target and abuse DLLs because they provide a powerful way to evade detection, maintain persistence, and bypass security controls by hijacking how Windows naturally runs applications.

**🚩 Answer Clue:** <span style="font-size: 1.50em;">**`R*******`**</span>

### Question 7: Sunny identified tcpdump on one of the compromised hosts. Assuming this was placed there by the threat actor, which technique might the APT be using here for discovery?

This question moves to another phase on the lifecycle: Discovery

To answer this question, we have to know what a tcpdump is. tcpdump is a powerful, lightweight command-line packet analyzer that captures and displays network traffic in real time. When something is captured, it means it's being logged, recorded and intercepted for further analysis. Another term for this is network sniffing. 

**🚩 Answer Clue:** <span style="font-size: 1.50em;">**`N****** S*******`**</span>

### Question 8: It looks like the APT achieved lateral movement by exploiting remote services. Which remote services should Sunny observe to identify APT activity traces?
Remote services are network-accessible interfaces, tools, or protocols that let users and administrators access and manage systems virtually. This can include cloud services, terminal protocols like SSH, or remote access protocols like VNC, WinRM, and RDP.

**🚩 Answer Clue:** <span style="font-size: 1.50em;">**`S**/W****** A**** S*****`**</span>

### Question 9: It looked like the primary goal of the APT was to steal intellectual property from E-corp's information repositories. Which information repository can be the likely target of the APT?
