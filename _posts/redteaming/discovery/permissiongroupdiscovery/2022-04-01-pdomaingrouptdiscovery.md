---
title: Permission Groups Discovery -> Domain Groups
date: 2022-04-01 05:49:33 +0800
categories: [Red Teaming, Discovery]
tags: []  
---

# Permission Groups Discovery : Domain Groups

- ID : `T1069.002`
- Tactic : `Discovery`
- Platforms: `Windows`, `linux`, `macos`

# Domain Groups

Adversaries may attempt to find domain-level groups and permission settings. The knowledge of domain-level permission groups can help adversaries determine which groups exist and which users belong to a particular group. Adversaries may use this information to determine which users have elevated permissions, such as domain administrators.

# Exploitations

## Windows

### Command Prompt

We will use command prompt to list the members of a specific group. To do that, we will have to get the list of available groups on the system and then choose the one that looks interesting to us i.e `Domain Admins`. We will execute `net group /domain`  to get domain groups, and then `net group "Domain Admins" /domain` to get the members of the `domain admins`.

```batch

net group /domain
net group "Domain Admins" /domain

```

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/pdomaingroup1.png)

As you can see, we were able to get the members of "Domain Admins" group.

### Powerview

We can get thesame result with powerview. Import powerview script and execute the below command.

```powershell

get-netgroupmember "Domain Admins" | select MembernName

```

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/pdomaingroup3.png)

As you can see, we were able to get the members of "Domain Admins" group.


# References

- [https://attack.mitre.org/techniques/T1069/001/](https://attack.mitre.org/techniques/T1069/002/)
- [https://atomicredteam.io/discovery/T1069.001/](https://atomicredteam.io/discovery/T1069.002/)
