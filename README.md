# SysmonForLinux - Configuration Manual
This repo contains specific configuration files for better understanding of sysmon configuration on Linux systems.

## Activating a configuration file:
You can activate following configuration files by:
```bash
sudo sysmon -c <xml_file>
```
## SysmonForLinux Events & Description Fields:
As you know, [SysmonForLinux](https://github.com/Sysinternals/SysmonForLinux) is just released and supports only 8 events from sysmon:

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

Example default configuration file: [processCreate.xml](https://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/processCreate.xml)


### ▶ Event ID 3 ➡ NetworkConnect
This event logs TCP/UDP connections on the machine. 

All Description Fields:
| ⬇  | ⬇  | ⬇  | ⬇  |  ⬇ |
|---|---|---|---|---|
| **RuleName**  |  **UtcTime** |  **ProcessGuid** |**ProcessId**|  **Image** |
|  **User** |  **Protocol** |  **Initiated** |  **SourceIsIpv6** | **SourceIp**  |
| **SourceHostname**  | **SourcePort**  |  **SourcePortName** | **DestinationIsIpv6**  | **DestinationIp**  |
| -  | **DestinationHostname**  |  **DestinationPort** | **DestinationPortName**  | -  |

Example default configuration file: [networkConnections.xml](https://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/networkConnections.xml)

### ▶ Event ID 5 ➡ ProcessTerminate
This event provides information about newly terminated processes.

All Description Fields:
| ⬇  | ⬇  | ⬇  | ⬇  |  ⬇ |
|---|---|---|---|---|
| **RuleName**  |  **UtcTime** |  **ProcessGuid** |**ProcessId**|  **Image** |

Example default configuration file: [processTerminate.xml](https://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/processTerminate.xml)

- Event ID 3  => NetworkConnect
- Event ID 5  => ProcessTerminate
- Event ID 9  => RawAccessRead
- Event ID 10 => ProcessAccess
- Event ID 11 => FileCreate
- Event ID 16 => Sysmon Config Change
- Event ID 23 => FileDelete


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

### only_ping.xml [gist](https://gist.github.com/oz9un/534a161a377f82f4d8d69dcba3e00ce0):
This configuration file only logs 'ping' actions.

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
