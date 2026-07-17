{
  "_index": "wazuh-archives-4.x-2026.07.15",
  "_id": "x_BjZZ8BPMbtb2uruXT_",
  "_score": 1,
  "_source": {
    "agent": {
      "ip": "192.168.92.134",
      "name": "SOC-Win10",
      "id": "001"
    },
    "manager": {
      "name": "wazuh-server"
    },
    "data": {
      "win": {
        "eventdata": {
          "originalFileName": "whoami.exe",
          "image": "C:\\\\Windows\\\\System32\\\\whoami.exe",
          "product": "Microsoft® Windows® Operating System",
          "parentProcessGuid": "{6c09860d-6543-6a57-d101-000000002900}",
          "description": "whoami - displays logged on user information",
          "logonGuid": "{6c09860d-6068-6a57-4524-050000000000}",
          "parentCommandLine": "\\\"C:\\\\Windows\\\\system32\\\\cmd.exe\\\"",
          "processGuid": "{6c09860d-6549-6a57-d401-000000002900}",
          "logonId": "0x52445",
          "parentProcessId": "6208",
          "processId": "3200",
          "currentDirectory": "C:\\\\Users\\\\Oxhun\\\\",
          "utcTime": "2026-07-15 10:47:37.594",
          "hashes": "MD5=A4A6924F3EAF97981323703D38FD99C4,SHA256=1D4902A04D99E8CCBFE7085E63155955FEE397449D386453F6C452AE407B8743,IMPHASH=7FF0758B766F747CE57DFAC70743FB88",
          "parentImage": "C:\\\\Windows\\\\System32\\\\cmd.exe",
          "company": "Microsoft Corporation",
          "commandLine": "whoami",
          "integrityLevel": "Medium",
          "fileVersion": "10.0.19041.1 (WinBuild.160101.0800)",
          "user": "DESKTOP-8GB0J2A\\\\Oxhun",
          "terminalSessionId": "1",
          "parentUser": "DESKTOP-8GB0J2A\\\\Oxhun"
        },
        "system": {
          "eventID": "1",
          "keywords": "0x8000000000000000",
          "providerGuid": "{5770385f-c22a-43e0-bf4c-06f5698ffbd9}",
          "level": "4",
          "channel": "Microsoft-Windows-Sysmon/Operational",
          "opcode": "0",
          "message": "\"Process Create:\r\nRuleName: -\r\nUtcTime: 2026-07-15 10:47:37.594\r\nProcessGuid: {6c09860d-6549-6a57-d401-000000002900}\r\nProcessId: 3200\r\nImage: C:\\Windows\\System32\\whoami.exe\r\nFileVersion: 10.0.19041.1 (WinBuild.160101.0800)\r\nDescription: whoami - displays logged on user information\r\nProduct: Microsoft® Windows® Operating System\r\nCompany: Microsoft Corporation\r\nOriginalFileName: whoami.exe\r\nCommandLine: whoami\r\nCurrentDirectory: C:\\Users\\Oxhun\\\r\nUser: DESKTOP-8GB0J2A\\Oxhun\r\nLogonGuid: {6c09860d-6068-6a57-4524-050000000000}\r\nLogonId: 0x52445\r\nTerminalSessionId: 1\r\nIntegrityLevel: Medium\r\nHashes: MD5=A4A6924F3EAF97981323703D38FD99C4,SHA256=1D4902A04D99E8CCBFE7085E63155955FEE397449D386453F6C452AE407B8743,IMPHASH=7FF0758B766F747CE57DFAC70743FB88\r\nParentProcessGuid: {6c09860d-6543-6a57-d101-000000002900}\r\nParentProcessId: 6208\r\nParentImage: C:\\Windows\\System32\\cmd.exe\r\nParentCommandLine: \"C:\\Windows\\system32\\cmd.exe\" \r\nParentUser: DESKTOP-8GB0J2A\\Oxhun\"",
          "version": "5",
          "systemTime": "2026-07-15T10:47:37.6113653Z",
          "eventRecordID": "49324",
          "threadID": "4720",
          "computer": "DESKTOP-8GB0J2A",
          "task": "1",
          "processID": "2556",
          "severityValue": "INFORMATION",
          "providerName": "Microsoft-Windows-Sysmon"
        }
      }
    },
    "decoder": {
      "name": "windows_eventchannel"
    },
    "full_log": "{\"win\":{\"system\":{\"providerName\":\"Microsoft-Windows-Sysmon\",\"providerGuid\":\"{5770385f-c22a-43e0-bf4c-06f5698ffbd9}\",\"eventID\":\"1\",\"version\":\"5\",\"level\":\"4\",\"task\":\"1\",\"opcode\":\"0\",\"keywords\":\"0x8000000000000000\",\"systemTime\":\"2026-07-15T10:47:37.6113653Z\",\"eventRecordID\":\"49324\",\"processID\":\"2556\",\"threadID\":\"4720\",\"channel\":\"Microsoft-Windows-Sysmon/Operational\",\"computer\":\"DESKTOP-8GB0J2A\",\"severityValue\":\"INFORMATION\",\"message\":\"\\\"Process Create:\\r\\nRuleName: -\\r\\nUtcTime: 2026-07-15 10:47:37.594\\r\\nProcessGuid: {6c09860d-6549-6a57-d401-000000002900}\\r\\nProcessId: 3200\\r\\nImage: C:\\\\Windows\\\\System32\\\\whoami.exe\\r\\nFileVersion: 10.0.19041.1 (WinBuild.160101.0800)\\r\\nDescription: whoami - displays logged on user information\\r\\nProduct: Microsoft® Windows® Operating System\\r\\nCompany: Microsoft Corporation\\r\\nOriginalFileName: whoami.exe\\r\\nCommandLine: whoami\\r\\nCurrentDirectory: C:\\\\Users\\\\Oxhun\\\\\\r\\nUser: DESKTOP-8GB0J2A\\\\Oxhun\\r\\nLogonGuid: {6c09860d-6068-6a57-4524-050000000000}\\r\\nLogonId: 0x52445\\r\\nTerminalSessionId: 1\\r\\nIntegrityLevel: Medium\\r\\nHashes: MD5=A4A6924F3EAF97981323703D38FD99C4,SHA256=1D4902A04D99E8CCBFE7085E63155955FEE397449D386453F6C452AE407B8743,IMPHASH=7FF0758B766F747CE57DFAC70743FB88\\r\\nParentProcessGuid: {6c09860d-6543-6a57-d101-000000002900}\\r\\nParentProcessId: 6208\\r\\nParentImage: C:\\\\Windows\\\\System32\\\\cmd.exe\\r\\nParentCommandLine: \\\"C:\\\\Windows\\\\system32\\\\cmd.exe\\\" \\r\\nParentUser: DESKTOP-8GB0J2A\\\\Oxhun\\\"\"},\"eventdata\":{\"utcTime\":\"2026-07-15 10:47:37.594\",\"processGuid\":\"{6c09860d-6549-6a57-d401-000000002900}\",\"processId\":\"3200\",\"image\":\"C:\\\\\\\\Windows\\\\\\\\System32\\\\\\\\whoami.exe\",\"fileVersion\":\"10.0.19041.1 (WinBuild.160101.0800)\",\"description\":\"whoami - displays logged on user information\",\"product\":\"Microsoft® Windows® Operating System\",\"company\":\"Microsoft Corporation\",\"originalFileName\":\"whoami.exe\",\"commandLine\":\"whoami\",\"currentDirectory\":\"C:\\\\\\\\Users\\\\\\\\Oxhun\\\\\\\\\",\"user\":\"DESKTOP-8GB0J2A\\\\\\\\Oxhun\",\"logonGuid\":\"{6c09860d-6068-6a57-4524-050000000000}\",\"logonId\":\"0x52445\",\"terminalSessionId\":\"1\",\"integrityLevel\":\"Medium\",\"hashes\":\"MD5=A4A6924F3EAF97981323703D38FD99C4,SHA256=1D4902A04D99E8CCBFE7085E63155955FEE397449D386453F6C452AE407B8743,IMPHASH=7FF0758B766F747CE57DFAC70743FB88\",\"parentProcessGuid\":\"{6c09860d-6543-6a57-d101-000000002900}\",\"parentProcessId\":\"6208\",\"parentImage\":\"C:\\\\\\\\Windows\\\\\\\\System32\\\\\\\\cmd.exe\",\"parentCommandLine\":\"\\\\\\\"C:\\\\\\\\Windows\\\\\\\\system32\\\\\\\\cmd.exe\\\\\\\"\",\"parentUser\":\"DESKTOP-8GB0J2A\\\\\\\\Oxhun\"}}}",
    "input": {
      "type": "log"
    },
    "@timestamp": "2026-07-15T10:47:38.584Z",
    "location": "EventChannel",
    "id": "1784112458.668554",
    "timestamp": "2026-07-15T10:47:38.584+0000"
  },
  "fields": {
    "timestamp": [
      "2026-07-15T10:47:38.584Z"
    ],
    "@timestamp": [
      "2026-07-15T10:47:38.584Z"
    ]
  }
}