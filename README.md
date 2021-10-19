# SysmonForLinux - Configuration Manual
This repo contains specific configuration files for better understanding of sysmon configuration on Linux systems.

<p>&nbsp;</p>

## Activating a configuration file:
You can activate following configuration files by:
```bash
sudo sysmon -c <xml_file>
```

<p>&nbsp;</p>

## SysmonForLinux Events & Description Fields:
As you know, [SysmonForLinux](https://github.com/Sysinternals/SysmonForLinux) is just released and supports limited events from sysmon. These are the most critical and popular ones:

<p>&nbsp;</p>

### ▶ Event ID 1 ➡ ProcessCreate
This event provides extended information about newly created processes.

All Description Fields:
| ⬇  | ⬇  | ⬇  | ⬇  |  ⬇ |
|---|---|---|---|---|
| **RuleName**  |  **UtcTime** |  **ProcessGuid** |**ProcessId**|  **Image** |
|  **FileVersion** |  **Description** |  **Product** |  **Company** | **OriginalFileName**  |
| **CommandLine**  | **CurrentDirectory**  |  **User** | **LogonGuid**  | **LogonId**  |
| **ParentProcessGuid** | **TerminalSessionId**  |  **IntegrityLevel** | **Hashes**  | **ParentProcessId**  |
| - | **ParentImage**  |  - | **ParentCommandLine**  | -  |

**Example default configuration file: [processCreate.xml](https://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/processCreate.xml)**

<p>&nbsp;</p>

### ▶ Event ID 3 ➡ NetworkConnect
This event logs TCP/UDP connections on the machine. 

All Description Fields:
| ⬇  | ⬇  | ⬇  | ⬇  |  ⬇ |
|---|---|---|---|---|
| **RuleName**  |  **UtcTime** |  **ProcessGuid** |**ProcessId**|  **Image** |
|  **User** |  **Protocol** |  **Initiated** |  **SourceIsIpv6** | **SourceIp**  |
| **SourceHostname**  | **SourcePort**  |  **SourcePortName** | **DestinationIsIpv6**  | **DestinationIp**  |
| -  | **DestinationHostname**  |  **DestinationPort** | **DestinationPortName**  | -  |

**Example default configuration file: [networkConnections.xml](https://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/networkConnections.xml)**

<p>&nbsp;</p>

### ▶ Event ID 5 ➡ ProcessTerminate
This event provides information about newly terminated processes.

All Description Fields:
| ⬇  | ⬇  | ⬇  | ⬇  |  ⬇ |
|---|---|---|---|---|
| **RuleName**  |  **UtcTime** |  **ProcessGuid** |**ProcessId**|  **Image** |

**Example default configuration file: [processTerminate.xml](https://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/processTerminate.xml)**

<p>&nbsp;</p>

### ▶ Event ID 9 ➡ RawAccessRead
This event detects reading operations mostly used by malwares for data exfiltration.

All Description Fields:
| ⬇  | ⬇  | ⬇  | ⬇  |  ⬇ |
|---|---|---|---|---|
| **RuleName**  |  **UtcTime** |  **ProcessGuid** |**ProcessId**|  **Image** |

**Example default configuration file: [rawAccessRead.xml](hhttps://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/rawAccessRead.xml)**

<p>&nbsp;</p>

### ▶ Event ID 10 ➡ ProcessAccess
This event detects when a process is accessed by another process and returns extended information about that event. This description is useful when detecting hacking tools that need to access specific system processes.

All Description Fields:
| ⬇  | ⬇  | ⬇  | ⬇  |  ⬇ |
|---|---|---|---|---|
| **RuleName**  |  **UtcTime** |  **SourceProcessGUID** |**SourceProcessId**|  **SourceThreadId** |
|  **SourceImage** |  **TargetProcessGUID** |  **TargetProcessId** |  **TargetImage** | **GrantedAccess**  |
| -  | -  |  **CallTrace** | -  | -  |

**Example default configuration file: [processAccess.xml](https://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/processAccess.xml)**

<p>&nbsp;</p>

### ▶ Event ID 11 ➡ FileCreate
This event detects when file is *created* or *overwrited*. This event can be useful when observing auto-startup folders, which are the favorite folder for malicious activites.
    
All Description Fields:
| ⬇  | ⬇  | ⬇  | ⬇  |  ⬇ |
|---|---|---|---|---|
| **RuleName**  |  **UtcTime** |  **ProcessGuid** |**ProcessId**|  **Image** |
|  -|  **TargetFilename** |  - |  **CreationUtcTime** | -  |
    

**Example default configuration file: [fileCreate.xml](https://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/fileCreate.xml)**

<p>&nbsp;</p>

### ▶ Event ID 16 ➡ FileDelete
This event detects when file is *shredded* or *deleted*.
    
All Description Fields:
| ⬇  | ⬇  | ⬇  | ⬇  |  ⬇ |
|---|---|---|---|---|
| **RuleName**  |  **UtcTime** |  **ProcessGuid** |**ProcessId**|  **User** |
|  **Image** |  **Hashes** |  - |  **IsExecutable** | **Archived**  |
    

**Example default configuration file: [fileDelete.xml](https://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/fileDelete.xml)**

<p>&nbsp;</p>

## Disabling logs for a specific event:
And logs are stored in /var/log/syslog by default (path cannot be edited yet 😑)

When I want to test something on my configuration files; I don't want to lost in huge log mess.

Therefore, I am disabling all other events.

For example, you can disable all ProcessTerminate events by:

```xml
    <!-- Event ID 5 == ProcessTerminate. Do not log anything! -->
    <RuleGroup name="" groupRelation="or">
      <ProcessTerminate onmatch="include"/>
    </RuleGroup>
```
All other logs can be disabled with the use of 'include' nothing (example above).

## Configuration Files for Template:


### all_disabled.xml [gist](https://gist.github.com/oz9un/95bda6a6c8be54df1a976a93eb6b8308):
This configuration can be used as a main template. 

All events are disabled in this configuration file.

Can be edited for specific purposes.

<p>&nbsp;</p>

### only_ping.xml [gist](https://gist.github.com/oz9un/534a161a377f82f4d8d69dcba3e00ce0):
This configuration file only logs 'ping' actions.

<p>&nbsp;</p>

### Network Specifications (network_specifications.xml) [gist](https://gist.github.com/oz9un/079114d034fb93d6dce22e1d0441d2cc):
You can limit your network connections by specifically logging them.

Example scenario:
- Allow only tcp and udp protocols.
- Allow only 80 and 443 ports.

It can be expressed as: (protocol:tcp || protocol:udp) && (port:80 || port:443) 

Expression above should outcome as True.

Valid configuration for that purpose should contain:
```xml
    <RuleGroup name="" groupRelation="and">
        <NetworkConnect onmatch="exclude">
                <Protocol condition="is">tcp</Protocol>
                <Protocol condition="is">udp</Protocol>
                <DestinationPort condition="is">80</DestinationPort>
                <DestinationPort condition="is">443</DestinationPort>
        </NetworkConnect>
    </RuleGroup>
```
**groupRelation="and"** is the critical part of that configuration.
