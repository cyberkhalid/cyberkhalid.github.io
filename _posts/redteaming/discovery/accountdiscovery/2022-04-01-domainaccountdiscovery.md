---
title: Domain Account
date: 2022-04-01 05:49:33 +0800
categories: [Red Teaming, Discovery]
tags: []  
---

# Domain Account

- ID : `T1087.002`
- Tactic : `Discovery`
- Platforms: `Windows`, `linux`, `macos`

# Local Account 

Adversaries may attempt to get a listing of domain accounts. This information can help adversaries determine which domain accounts exist to aid in follow-on behavior.

# Exploitations

## Windows

### Command Prompt

#### Users

We can list `domain user accounts` in windows os by executing the following command in command prompt or powershell.

```batch

net user /domain

```

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/domainaccount1.png)

Above image shows the listing of domain user accounts.

#### Groups

We can list `domain groups` in windows os by executing the following command in command prompt or powershell.

```batch

net group /domain

```

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/domainaccount7.png)

Above image shows the listing of domain groups.

### Powerview

#### Users

We can use powerview to list `domain user accounts` in windows os. Import powerview script and execute the below command.

```powershell

get-netuser | select name

```
![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/domainaccount5.png)

Above image shows the listing of domain user accounts.

#### Groups

We can use powerview to list `domain groups` in windows os. Import powerview script and execute the below command.

```powershell

get-netgroup | select samaccountname

```
![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/domainaccount6.png)

Above image shows the listing of domain groups.

# References

- [https://attack.mitre.org/techniques/T1087/002/](https://attack.mitre.org/techniques/T1087/002/)
- [https://atomicredteam.io/discovery/T1087.002/](https://atomicredteam.io/discovery/T1087.002/)
