# SysmonForLinux-Configs
This repo contains specific configuration files for better understanding of sysmon configuration on Linux systems.

## Disabling logs for a specific event:
As you know, [SysmonForLinux](https://github.com/Sysinternals/SysmonForLinux) is just released and supports only 6 events from sysmon:
- Event ID 3 == NetworkConnect
- Event ID 5 == ProcessTerminate
- Event ID 9 == RawAccessRead
- Event ID 10 == ProcessAccess
- Event ID 11 == FileCreate
- Event ID 23 == FileDelete

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


## only_ping.xml [gist](https://gist.github.com/oz9un/534a161a377f82f4d8d69dcba3e00ce0):
This configuration file only logs 'ping' actions.
