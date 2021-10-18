# SysmonForLinux-Configs
This repo contains specific configuration files for better understanding of sysmon configuration on Linux systems.

## only_ping.xml:
This configuration file only logs 'ping' actions.

All other logs are disabled with the use of 'include' nothing.

For example, you can disable all ProcessTerminate events by:

```xml
    <!-- Event ID 5 == ProcessTerminate. Do not log anything! -->
    <RuleGroup name="" groupRelation="or">
      <ProcessTerminate onmatch="include"/>
    </RuleGroup>
```

Point is escaping from the log mess.
