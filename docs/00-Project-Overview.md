Enterprise-SOC-Lab/
│
├── README.md
├── LICENSE
├── .gitignore
├── CHANGELOG.md
├── CONTRIBUTING.md
│
├── docs/
│   │
│   ├── 00-Project-Overview.md
│   ├── 01-Lab-Architecture.md
│   ├── 02-Prerequisites.md
│   ├── 03-Virtual-Lab-Setup.md
│   ├── 04-Ubuntu-Server-Installation.md
│   ├── 05-Windows-10-Installation.md
│   ├── 06-Kali-Linux-Installation.md
│   ├── 07-Wazuh-Installation.md
│   ├── 08-Windows-Agent-Deployment.md
│   ├── 09-Sysmon-Installation.md
│   ├── 10-Wazuh-Sysmon-Integration.md
│   ├── 11-Understanding-Sysmon.md
│   ├── 12-Understanding-Wazuh.md
│   ├── 13-Understanding-Logs.md
│   ├── 14-Understanding-SIEM.md
│   ├── 15-Troubleshooting.md
│   ├── 16-Lessons-Learned.md
│   └── 17-References.md
│
├── investigations/
│   │
│   ├── 00-Investigation-Guide.md
│   ├── Investigation-01-Whoami.md
│   ├── Investigation-02-Hostname.md
│   ├── Investigation-03-Systeminfo.md
│   ├── Investigation-04-Ipconfig.md
│   ├── Investigation-05-Arp.md
│   ├── Investigation-06-Route.md
│   ├── Investigation-07-Netstat.md
│   ├── Investigation-08-NSLookup.md
│   ├── Investigation-09-Ping.md
│   ├── Investigation-10-Net-User.md
│   ├── Investigation-11-Net-Localgroup.md
│   ├── Investigation-12-Tasklist.md
│   ├── Investigation-13-SC-Query.md
│   ├── Investigation-14-Net-Start.md
│   ├── Investigation-15-DriverQuery.md
│   ├── Investigation-16-Mkdir.md
│   ├── Investigation-17-Echo.md
│   ├── Investigation-18-Copy.md
│   ├── Investigation-19-Move.md
│   ├── Investigation-20-Del.md
│   ├── Investigation-21-Reg-Query.md
│   ├── Investigation-22-Get-Process.md
│   ├── Investigation-23-Get-Service.md
│   ├── Investigation-24-Get-ComputerInfo.md
│   ├── Investigation-25-Get-NetIPAddress.md
│   └── Investigation-Summary.md
│
├── attacks/
│   │
│   ├── Attack-01-Nmap.md
│   ├── Attack-02-RustScan.md
│   ├── Attack-03-Hydra.md
│   ├── Attack-04-BruteForce.md
│   ├── Attack-05-PowerShell.md
│   ├── Attack-06-Reverse-Shell.md
│   ├── Attack-07-Mimikatz.md
│   ├── Attack-08-Persistence.md
│   ├── Attack-09-Lateral-Movement.md
│   └── Attack-Summary.md
│
├── detection-rules/
│   │
│   ├── Custom-Wazuh-Rules.md
│   ├── MITRE-Mapping.md
│   ├── Detection-Logic.md
│   └── False-Positive-Tuning.md
│
├── reports/
│   │
│   ├── Incident-Report-Template.md
│   ├── Investigation-Report-Template.md
│   ├── IOC-Report.md
│   └── Daily-SOC-Notes.md
│
├── commands/
│   │
│   ├── Ubuntu-Commands.md
│   ├── Windows-Commands.md
│   ├── Sysmon-Commands.md
│   ├── Wazuh-Commands.md
│   ├── OpenSearch-Commands.md
│   └── PowerShell-Commands.md
│
├── diagrams/
│   │
│   ├── Lab-Architecture.drawio
│   ├── Network-Diagram.drawio
│   ├── Data-Flow.drawio
│   ├── Investigation-Workflow.drawio
│   └── Attack-Workflow.drawio
│
├── screenshots/
│   │
│   ├── architecture/
│   ├── vmware/
│   ├── ubuntu/
│   ├── wazuh/
│   ├── windows-agent/
│   ├── sysmon/
│   ├── event-viewer/
│   ├── dashboard/
│   ├── investigations/
│   ├── attacks/
│   └── troubleshooting/
│
├── scripts/
│   │
│   ├── ubuntu/
│   ├── windows/
│   ├── powershell/
│   └── wazuh/
│
└── assets/
    │
    ├── icons/
    ├── banners/
    └── logos/