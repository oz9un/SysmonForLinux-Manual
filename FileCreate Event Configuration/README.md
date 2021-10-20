In this article, I will explain how to use SysmonForLinux and how to create specific configurations to keep track of file creation events.

For further information about events and configuration options; you can visit my github repository: [oz9un](https://github.com/oz9un/SysmonForLinux-Manual)

<p>&nbsp;</p>

## Sysmon in a nutshell:

**Sysmon** (System Monitor) is a Windows system service that logs system activity to the Windows event log. Now, it is available for **Linux** too! 

It was developed by Microsoft as an open source project. Even though it is doubtful to see such a contribution, I am very glad about it 🤭

![sus_gif](https://media4.giphy.com/media/ANbD1CCdA3iI8/giphy.gif?cid=ecf05e47chlsv1aaptk03nsstucjmbytwln7w8t9locgvmfi&rid=giphy.gif&ct=g)

<p>&nbsp;</p>

## File Create event:
As its name suggests, this event detects when a file is created on Linux systems.

This event can be especially useful when tracking auto-startup folders and folders where everyone has a permission to create files. Because most of the time, they are the main targets of malicious users 👹.

A normal file creation event log would looks like this:
![FileCreation Event Log](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4x8zf6qaw3r9m1164w7d.png)

<p>&nbsp;</p>

## Configuring File Create event:
By default, SysmonForLinux logs every file creation events. But we are talking about Linux. Even if you connect to a server via SSH; that results in the creation of many tmp files.

As a result, you can guess how chaotic the log file has become. This is why we don't want to log everything, especially in case of file creation event.

File Create event has 7 different description fields that we can use as filters:
![Description Fields](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bw6rxk7loe0i65y60rx1.png)

You can view all description fields for each event on my [github](https://github.com/oz9un/SysmonForLinux-Manual) page!

When creating a configuration file (xml) for SysmonForLinux, we can use this fields as a filter.

<p>&nbsp;</p>

## Example scenario:
Suppose you want to keep log of the every file creation event with the following properties:
- File should be created in */tmp* or under */home/secret* folder.
- File should be created in *October 2021*.
- File shouldn't be created with */usr/sbin/cups-browsed* daemon (This printer daemon uses /tmp file quite often).

Desired configuration file should looks like that:
```xml
<!-- Event ID 11 == FileCreate. Log what we want! -->
<RuleGroup name="" groupRelation="and">
  <FileCreate onmatch="include">
    <TargetFilename condition="contains any">/home/secret/;/tmp/</TargetFilename>
    <UtcTime condition="contains">2021-10</UtcTime>
  </FileCreate>

  <FileCreate onmatch="exclude">
    <Image condition="is">/usr/sbin/cups-browsed</Image>
  </FileCreate>
</RuleGroup>
```

You can view the whole configuration file from: [Example Scenario](https://github.com/oz9un/SysmonForLinux-Manual/blob/main/Description%20Field%20Examples/FileCreate_ExampleScenario.xml)

<p>&nbsp;</p>

## Results:
To view sysmon's log file:
```bash
tail -f /var/log/syslog | sudo /opt/sysmon/sysmonLogView -X
```
Output:

![Sysmon Log Output](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h3ujafruawscruow8w2c.png)
 
As you can see, we successfully created a configuration file according to our properties and caught some malicious events already!

![hacker_gif](https://media4.giphy.com/media/TOWeGr70V2R1K/giphy.gif?cid=ecf05e47a1op0mndh2ygfi3sf7fx21ioab3rxnp60jdeezsn&rid=giphy.gif&ct=g)

<p>&nbsp;</p>

## End & Contact:
This is at the end of my first post about SysmonForLinux. I plan to write much more on this subject as I find time. I would be very happy if you can give a feedback. Stay in touch!

Github: https://github.com/oz9un
Twitter: https://twitter.com/oz9un

![cat_gif](https://media4.giphy.com/media/iPiUxztIL4Sl2/giphy.gif?cid=ecf05e47u8cqoaovibilphanyitfewmsnw8wa7c35tqxez0p&rid=giphy.gif&ct=g)

 
 

