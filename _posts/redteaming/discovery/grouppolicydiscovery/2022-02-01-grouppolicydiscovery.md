---
title: Group Policy Discovery 
date: 2022-04-01 05:49:33 +0800
categories: [Red Teaming, Discovery]
tags: []  
---

# Group Policy Discovery 

- ID : `T1615`
- Tactic : `Discovery`
- Platforms: `Windows`

# Group Policy Discovery 

Adversaries may gather information on Group Policy settings to identify paths for privilege escalation, security measures applied within a domain, and to discover patterns in domain objects that can be manipulated or used to blend in the environment. Group Policy allows for centralized management of user and computer settings in Active Directory (AD).

# Exploitations

## Windows

### Command Prompt

We can use `gpresult` to get the Group Policy Setting applied on a domain object i.e `user object, computer object`. 

```batch

gpresult /R /V

```
![process](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/gp1.png)

### Powerview

We can use powerview to get the list of group policy objects within a domain. Import powerview script and then execute the below command.

```powershell

get-netgpo | select displayname, cn

```

![process](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/gp2.png)

# References

- [https://attack.mitre.org/techniques/T1615/](https://attack.mitre.org/techniques/T1615/)

