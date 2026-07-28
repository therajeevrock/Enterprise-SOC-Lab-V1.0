# Summary

This investigation series explored common Windows command-line utilities using real Sysmon and Wazuh telemetry collected in the Enterprise SOC Lab. Each command was executed from both Command Prompt and Windows PowerShell, allowing analysts to examine process creation events, parent-child process relationships, Wazuh detection rules, and MITRE ATT&CK mappings.

Throughout these investigations, it became clear that legitimate administrative commands can also appear during attacker reconnaissance. For this reason, SOC analysts should always evaluate the execution context, user activity, process hierarchy, and related events before determining whether an alert represents normal administration or suspicious behavior.

This series provides a practical foundation for Windows process investigations and prepares analysts for more advanced investigations involving attacker techniques, persistence, credential access, lateral movement, and other real-world attack scenarios.