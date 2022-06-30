---
title: Process Discovery 
date: 2022-04-01 05:49:33 +0800
categories: [Red Teaming, Discovery]
tags: [redteam,linux, windows, mitre, process discovery , process,discovery]     # TAG names should always be lowercase
---

# Process Discovery 

- ID : `T1057`
- Tactic : `Discovery`
- Platforms: `Windows`, `linux`, `macos`

# Process Discovery 

Adversaries may attempt to get information about running processes on a system. Information obtained could be used to gain an understanding of common software/applications running on systems within the network. Adversaries may use the information from Process Discovery during automated discovery to shape follow-on behaviors, including whether or not the adversary fully infects the target and/or attempts specific actions.

# Exploitations

## Windows

### Command Prompt

We can get the list of a `running processes` in windows by executing this command in command prompt.

```batch

tasklist

```
![process](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/process1.png)

### Powershell

We can get the list of a `running processes` in windows by executing this command in powershell.

```powershell

get-process

```

![process](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/process2.png)

## Linux

We can get the list of a `running processes` in linux by executing the below command.

```bash

ps aux

```
![process](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/process3.png)

# References

- [https://attack.mitre.org/techniques/T1057/](https://attack.mitre.org/techniques/T1057/)
- [https://atomicredteam.io/discovery/T1057/](https://atomicredteam.io/discovery/T1057/)
