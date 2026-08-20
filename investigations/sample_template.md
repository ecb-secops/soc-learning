# PowerShell Investigation #

## Scenario
Suspicious PowerShell activity detected.

## Evidence
- Parent process: winword.exe
- Child process: powershell.exe
- Encoded command observed

## MITRE ATT&CK
T1059.001 PowerShell

## Detection Opportunities
- Monitor EncodedCommand
- Alert on Office spawning PowerShell

## Lessons Learned
PowerShell launched from Office applications
should be investigated.
