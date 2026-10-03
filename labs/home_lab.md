# Home Lab Network Topology #
version 1.0 (will update once other OS is installed)

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/55fb8a03-b010-4d1b-98c1-c1bb99df725b" />

Corrections: 
                         ## Current Status

Network foundation is complete. Windows 11 and Ubuntu communicate through pfSense on an isolated VMware lab network. The next phase is Windows telemetry with Sysmon.

---

## 1. Final Network Topology

```text
                         INTERNET
                             |
                    Physical Host/Laptop
                             |
                       VMware Workstation
                             |
                      VMnet8 (NAT)
                             |
                       pfSense WAN
                    192.168.99.128
                             |
                    pfSense Firewall/Router
                             |
                       pfSense LAN
                    192.168.50.254/24
                             |
                    VMnet2 (Host-only)
                    192.168.50.0/24
                       /      |      \
                      /       |       \
                     v        v        v
              Windows 11   Ubuntu    Kali
              .50.101      .50.102   Future
```

**Important:** The actual pfSense LAN gateway is **192.168.50.254**. Use `.254`, not `.1`.

---

## 2. VMware Networking

In VMNet Network Editor:

If we want our VM (e.g., PFSense) to provide DHCP, uncheck **"Use local DHCP service to distribute IP address to VMs"**.

If this is checked. VMWare will act as the network's router and runs its own built-in DHCP server.

Checking the box to **"Connect a host virtual adapter to this network"** allows your physical host computer to communicate directly with your virtual machines.

When this option is enabled, VMware creates a virtual network card (like VMnet1 or VMnet8) inside your physical computer's operating system.

### VMnet8 — NAT

Used only for the pfSense WAN interface.

- Provides pfSense with Internet access through the host.
- pfSense WAN received `192.168.99.128/24` via DHCP.
- Lab VMs do not connect directly to VMnet8.

### VMnet2 — Host-only

Created specifically for the SOC lab.

```text
Type: Host-only
Subnet: 192.168.50.0/24
VMware DHCP: Disabled
Host virtual adapter: Enabled
```

All lab VMs connect to VMnet2.

---

## 3. pfSense

Installed as **pfSense Community Edition (CE)**.

### Interfaces

| Interface | VMware Network | Address |
|---|---|---|
| WAN | VMnet8 NAT | DHCP — `192.168.99.128` |
| LAN | VMnet2 Host-only | `192.168.50.254/24` |

### LAN DHCP

```text
Pool: 192.168.50.100 - 192.168.50.199
Gateway: 192.168.50.254
DNS: 192.168.50.254
```

pfSense is the only Internet gateway for the lab.

```text
Once pfSense is installed, setup the interfaces:
em0 -> WAN
em1 -> LAN
How to check:
VM -> Settings -> Network Adapter -> Settings

Choose '2' = Set Interface(s) IP
LAN -> 192.168.50.254
If subnet bit count is asked: 24
"Do you want to enable DHCP server on LAN?" -> N
  Range: 192.168.50.100 -> 192.168.50.199
Note: For all IPv6 questions, type 'N'
Enter the new LAN IPv4 upstream.... and click Enter and do not type anything.
```

---

## 4. Windows 11

Windows 11 was installed instead of the originally planned Windows 10.

Network:

```text
Adapter: VMnet2
IPv4: 192.168.50.101
Mask: 255.255.255.0
Gateway: 192.168.50.254
DHCP: 192.168.50.254
DNS: 192.168.50.254
```

Windows is the primary endpoint/victim for telemetry exercises.

### Bypass TPM Requirements ###

```text
1. On SETUP, when choosing language, hit SHIFT + F10 to launch CMD
2. Type 'regedit'
3. Go to HKLM\SYSTEM\SETUP
4. Create new registry key
  - "LabConfig"
5. Within LabConfig, create DWORD 32-bit value
    Key: "BypassTPMCheck"
    Value: 1
    Key: "BypassSecureBootCheck"
    Value: 1
6. Close regedit
```

### Bypass "Unlock Microsoft Experience" ###

```text
1. On SETUP, when choosing language, hit SHIFT + F10 to launch CMD
2. Type 'start ms-chx:localonly'
```

### Bypass "Let's connect you to network" ###

```text
1. On SETUP, when choosing language, hit SHIFT + F10 to launch CMD
2. Type 'OOBE\BypassNRO'
3. PC will restart automatically.
4. Go back to the internet connection screen and click "I don't have internet" to create a local user account.
```

Once done, test the connection:

1. Test IP -> should be in 192.168.50.100+ range (pfSense will distribute IP)
2. in CMD, ipconfig/all. 
  DNS, DHCP, Default Gateway should be 192.168.50.254
3. ping 192.168.50.254
4. ping 8.8.8.8 or google.com

---

## 5. Ubuntu

Ubuntu is connected to VMnet2 and received:

```text
192.168.50.102
```

Routing and DNS were checked with:

```bash
ip route
resolvectl status
```

Ubuntu can be suspended when it is not needed.

---

## 6. Error Encountered: Ubuntu Could Not Ping Windows

Initial result:

```text
Windows -> Ubuntu    WORKED
Ubuntu -> Windows    FAILED
```

The VMware network and pfSense configuration were correct.

The problem was **Windows Firewall** blocking inbound ICMP Echo Requests.

### Solution

Run PowerShell as Administrator on Windows:

```powershell
New-NetFirewallRule -DisplayName "Lab - Allow ICMPv4" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow
```

Meaning:

- `New-NetFirewallRule` — creates a Windows Firewall rule
- `-DisplayName` — gives it a readable name
- `-Protocol ICMPv4` — IPv4 ICMP
- `-IcmpType 8` — Echo Request (`ping`)
- `-Direction Inbound` — traffic entering Windows
- `-Action Allow` — permits matching traffic

After this rule was created, Ubuntu could ping Windows successfully.

### Lesson

A working connection in one direction does not guarantee the reverse direction will work. Host firewalls can independently block inbound traffic.

---

# 7. Why the Lab Is Designed This Way

The intended traffic flow is:

```text
Home/Office Network
        |
      NAT
        |
     pfSense WAN
        |
   pfSense Firewall
        |
     pfSense LAN
        |
   192.168.50.0/24
     /    |    \
 Windows Ubuntu Kali
```

The lab VMs are not bridged directly onto the physical network.

This gives us a controlled environment for:

- Windows telemetry
- firewall logs
- DNS
- network connections
- endpoint detection
- SIEM ingestion
- threat hunting
- controlled attack simulations

---

# 8. Next Phase: Windows Telemetry

The network foundation is now complete.

The next goal is to understand what Windows actually records when activity happens.

Learning path:

```text
Windows activity
      |
      v
Windows Event Logs
      |
      v
Sysmon
      |
      v
Process creation
      |
      v
Process trees
      |
      v
Normal baseline
      |
      v
Suspicious behavior
      |
      v
Splunk / SIEM
      |
      v
SPL hunting
      |
      v
MITRE ATT&CK
```

---

# 9. Event Viewer vs SIEM

Use **Event Viewer first**, then SIEM.

Event Viewer teaches what the raw evidence means.

Splunk teaches how to search and correlate that evidence at scale.

The goal is not to memorize every Event ID.

Instead, learn to answer:

```text
WHO?
WHAT?
WHO STARTED IT?
WHERE?
HOW?
WHEN?
```

For process investigations, important fields include:

- User
- Process name
- Parent process
- Parent command line
- Process command line
- Image path
- Process ID
- Parent Process ID
- Integrity level
- Hashes
- Network activity
- File activity

---

# 10. Process Tree Skill We Want to Build

For example:

```text
explorer.exe
    |
    +-- powershell.exe
```

means PowerShell had `explorer.exe` as its parent process.

Another execution could be:

```text
explorer.exe
    |
    +-- cmd.exe
          |
          +-- powershell.exe
```

The important lesson is not:

> PowerShell = malicious

Instead ask why that parent/child relationship exists.

For example:

```text
explorer.exe -> powershell.exe
```

can be normal interactive activity.

But:

```text
winword.exe -> powershell.exe
```

deserves investigation because the parent/child relationship is less expected.

Likewise:

```text
services.exe -> svchost.exe
```

is a common Windows relationship, while:

```text
services.exe -> powershell.exe
```

requires context.

**Do not classify a process as malicious from the process name alone.**

---

# 11. Sysmon — Next Exercise

The next major exercise is Sysmon, especially:

```text
Event ID 1 — Process Creation
```

Generate known activity:

1. Open PowerShell normally.
2. Open CMD normally.
3. Run PowerShell from CMD.
4. Find the Sysmon process-creation events.
5. Compare the parent/child relationships.

The objective is to reconstruct trees such as:

```text
explorer.exe
    |
    +-- powershell.exe
```

and:

```text
explorer.exe
    |
    +-- cmd.exe
          |
          +-- powershell.exe
```

Only after learning normal behavior should we deliberately create suspicious-looking activity.

---

# 12. Current Lab Inventory

| Device | Network | IP | Status |
|---|---|---|---|
| pfSense WAN | VMnet8 NAT | `192.168.99.128` | Active |
| pfSense LAN | VMnet2 | `192.168.50.254` | Active |
| Windows 11 | VMnet2 | `192.168.50.101` | Active |
| Ubuntu | VMnet2 | `192.168.50.102` | Available/suspendable |
| Kali | VMnet2 | Future | Not yet used |

---

# 13. Roadmap

```text
[✓] VMware networking
[✓] VMnet8 NAT
[✓] VMnet2 Host-only
[✓] pfSense installation
[✓] pfSense WAN
[✓] pfSense LAN
[✓] pfSense DHCP
[✓] Windows 11
[✓] Ubuntu
[✓] Windows <-> Ubuntu connectivity
[✓] Diagnose Windows ICMP blocking
[✓] Create Windows ICMP firewall rule

[→] Sysmon
[→] Windows Event Viewer
[→] Process Creation / Event ID 1
[→] Process trees
[→] Normal Windows baseline
[ ] PowerShell telemetry
[ ] Authentication telemetry
[ ] Network connection telemetry
[ ] Splunk
[ ] SPL
[ ] Controlled attack simulations
[ ] Threat hunting
[ ] MITRE ATT&CK mapping
[ ] Threat intelligence
[ ] Detection engineering
[ ] SOC investigation reports
```

---

# 14. Key Lessons So Far

### Network problems are not always network problems

The Windows/Ubuntu ping problem was caused by Windows Firewall, not VMware or pfSense.

### Direction matters

```text
Windows -> Ubuntu
```

working does not mean:

```text
Ubuntu -> Windows
```

must work.

### Know the gateway

The lab gateway is:

```text
192.168.50.254
```

### Keep the lab isolated

Lab VMs use:

```text
VMnet2
192.168.50.0/24
```

rather than being bridged directly onto the physical network.

### Learn telemetry from the bottom up

```text
Activity
  -> raw telemetry
  -> fields
  -> interpretation
  -> SIEM
  -> hunting
```

The goal is to develop contextual judgment rather than memorize a list of "good
