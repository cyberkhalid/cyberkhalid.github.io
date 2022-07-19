---
title: Permission Groups Discovery -> Local Groups 
date: 2022-04-01 05:49:33 +0800
categories: [Red Teaming, Discovery]
tags: []  
---

# Permission Groups Discovery : Local Groups 

- ID : `T1069.001`
- Tactic : `Discovery`
- Platforms: `Windows`, `linux`, `macos`

# Local Groups

Adversaries may attempt to find local system groups and permission settings. The knowledge of local system permission groups can help adversaries determine which groups exist and which users belong to a particular group. Adversaries may use this information to determine which users have elevated permissions, such as the users found within the local administrators group.

# Exploitations

## Windows

### Command Prompt

We will use command prompt to list the members of a specific group. To do that, we will have to get the list of available groups on the system and then choose the one that looks interesting to us i.e `Administrators`. Let's execute the following command to get the list of available groups. 

```batch

net localgroup

```

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount3.png)

As you can see , we were able to list the available groups on the system.

Now let's list the members of `Administrators` group since it has higher privileges than other groups. 

```batch

net localgroup Administrators

```

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/plocalaccount1.png)

As you can see, we were able to get the members of Administrators group.

### Powershell

We will use powershell to list the members of a specific group. To do that, we will have to get the list of available groups on the system and then choose the one that looks interesting to us i.e `Administrators`. Let's execute the following command to get the list of available groups.

```powershell

get-localgroup

```

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/plocalaccount2.png)

As you can see , we were able to list the available groups on the system.

Now let's list the members of `Administrators` group since it has higher privileges than other groups. 

```powershell

get-localgroupmember -Name Administrators

```

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/plocalaccount3.png)



# References

- [https://attack.mitre.org/techniques/T1069/001/](https://attack.mitre.org/techniques/T1069/001/)
- [https://atomicredteam.io/discovery/T1069.001/](https://atomicredteam.io/discovery/T1069.001/)
