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

**🚩 Answer:** <span style="font-size: 1.50em;">**`S****p******* L***`**</span>

### Question 2: Sunny identified that the APT might have moved forward from the recon phase. Which accounts might the APT compromise while developing resources?
Focusing on 'Resource Development', we can assume that the possible target of this APT group are email accounts. 

<img width="378" height="743" alt="image" src="https://github.com/user-attachments/assets/edb8cdf5-d3fc-4ca9-af1e-cfca7df13957" />

Keeping in mind that this group used spearphishing as a recon and initial access, spearphishing links are usually delivered through emails relying on account owners to unknowingly access the link to start the compromise.
This makes it a better answer than the other highlighted techniques. 

**🚩 Answer:** <span style="font-size: 1.50em;">**`E**** A*******`**</span>


