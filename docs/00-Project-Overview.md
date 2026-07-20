Enterprise-SOC-Lab
|
+--.gitignore
+---README.md
|
+---commands
|       learn.md
|       MITRE-Reference.md
|       Wazuh-Search-Queries.md
|       Windows-Command-Reference.md.md
|
+---diagrams
+---docs
|   |   00-Project-Overview.md
|   |   01-Lab-Architecture.md
|   |   02-Prerequisites.md
|   |   03-Virtual-Lab-Setup.md
|   |   04-Ubuntu-Server-Installation.md
|   |   05-Windows10-installation.md
|   |   06-Kali-Linux-Installation.md
|   |   07-Wazuh-Installation.md
|   |   08-Windows-Agent-Deployment.md
|   |   09-Sysmon-Installation.md
|   |   10-Wazuh-Sysmon-Integration.md
|   |   11-Understanding-Sysmon.md
|   |   12-Understanding-Wazuh.md
|   |   13-Understanding-logs.md
|   |   14-Understanding-SIEM.md
|   |   15-Troubleshooting.md
|   |   16-Lessons-Learned.md
|   |   17-References.md
|   |   documentation-write-rules.txt
|   |
|   \---Installation-Guide
|           05-Windows 10 Installation.md
|
+---investigations
|       00-Investigation-Guide.md
|       01-Investigation-Whoami.md
|       02-Investigation-Hostname.md
|
+---reports
\---screenshots
    +---integration
    |   |   01-ossec-conf.png
    |   |   02-sysmon-eventchannel.png
    |   |   03-agent-restart.png
    |   |   05-dashboard-events.png
    |   |   06-event-details.png
    |   |   07-end-to-end-verification.png
    |   |
    |   \---04-generate-test-events
    |           cmd.png
    |           folder create.png
    |           launch edge.png
    |           notepad.png
    |           powershell.png
    |
    +---investigations
    |   +---hostname
    |   |       01-command.png
    |   |       02-wazuh-event.png
    |   |       03-event-details.png
    |   |
    |   \---whoami
    |           01-command.png
    |           02-wazuh-event.png
    |           03-event-details.png
    |
    +---kali
    |       01-kali-vmware-download.png
    |       02-import-vm.png
    |       03-vm-hardware-settings.png
    |       04-virtualization-engine-settings.png
    |       05-first-login.png
    |       06-installation-verification.png
    |       07-system-update.png
    |
    +---sysmon
    |       01-sysmon-download.png
    |       02-sysmon-config-download.png
    |       03-sysmon-extracted-files.png
    |       04-sysmon-installation.png
    |       05-sysmon-configuration.png
    |       06-sysmon-service.png
    |       07-event-viewer.png
    |       08-test-event.png
    |       09-sysmon-installation-verified.png
    |
    +---ubuntu
    |       01-ubuntu-download-page.png
    |       02-vmware-new-vm.png
    |       03-vm-hardware-settings.png
    |       04-first-login.png
    |       05-system-update.png
    |
    +---wazuh
    |       01-dashboard-login.png
    |       02-dashboard-overview.png
    |       03-services-status.png
    |       04-network-verification.png
    |
    +---windows
    |       01-vm-files.png
    |       02-harware-setting.png
    |       03-first-login.png
    |
    \---windows-agent