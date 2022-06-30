---
title: Local Account 
date: 2022-04-01 05:49:33 +0800
categories: [Red Teaming, Discovery]
tags: [redteam,linux, windows, mitre, local account, discovery]     # TAG names should always be lowercase
---

# Local Account 
- ID : `T1087.001`
- Tactic : `Discovery`
- Platforms: `Windows`, `linux`, `macos`

# Local Account 

Adversaries may attempt to get a listing of local system accounts. This information can help adversaries determine which local accounts exist on a system to aid in follow-on behavior.

# Exploitations

## Windows

### Command Prompt

#### Users

We can list `local user accounts` in windows os by executing the following command in command prompt.

```batch

net user

```

or 

```batch

dir C:\Users\

```
![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount1.png)

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount2.png)

Above images show the listing of local user accounts.

#### Groups

We can list available `groups` in windows os by executing the following command in command prompt.

```batch

net localgroup

```
![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount3.png)

Above image shows the listing of groups.

### Powershell

#### Users

We can list `local user accounts` in windows os by executing the following command in powershell.

```powershell

get-localuser

```
![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount5.png)

Above image shows the listing of local user accounts.

#### Groups

We can list available `groups` in windows os by executing the following command in powershell.

```powershell

get-localgroup

```
![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount6.png)

Above image shows the listing of groups.

# Linux

## Users

We can list `local user/system accounts` in linux os by executing this command.

```bash

cat /etc/passwd

```

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount7.png)

Above image shows the listing of local user/system accounts.

## Groups

We can list available `groups` in linux os by executing this command.

```bash

groups

```

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount8.png)


# References

- [https://attack.mitre.org/techniques/T1087/001/](https://attack.mitre.org/techniques/T1087/001/)
- [https://atomicredteam.io/discovery/T1087.001/](https://atomicredteam.io/discovery/T1087.001/)
