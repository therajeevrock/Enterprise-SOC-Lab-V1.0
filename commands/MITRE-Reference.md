| Windows Command | MITRE ATT&CK ID | Technique | Tactic |
|-----------------|-----------------|-----------|---------|
| `whoami` | T1033 | System Owner / User Discovery | Discovery |
| `hostname` | T1082 | System Information Discovery | Discovery |
| `systeminfo` | T1082 | System Information Discovery | Discovery |
| `ipconfig` | T1016 | System Network Configuration Discovery | Discovery |
| `arp -a` | T1016 | System Network Configuration Discovery | Discovery |
| `route print` | T1016 | System Network Configuration Discovery | Discovery |
| `netstat -ano` | T1049 | System Network Connections Discovery | Discovery |
| `nslookup` | T1016 | System Network Configuration Discovery | Discovery |
| `tasklist` | T1057 | Process Discovery | Discovery |
| `net user` | T1087 | Account Discovery | Discovery |
| `net localgroup` | T1087 | Account Discovery | Discovery |
| `net localgroup` | T1069.001 | Permission Groups Discovery: Local Groups | Discovery |
| `net localgroup` *(PowerShell)* | T1059.001 | PowerShell | Execution |
* T1059.001 (PowerShell) applies only when the command is executed through Windows PowerShell, as detected by Wazuh Rule 92033. |