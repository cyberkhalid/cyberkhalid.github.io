---
title: Local Account
date: 2022-04-08 07:49:33 +0800
categories: [Red Teaming, Persistence]
tags: [redteam,persistence, create account, local account, mitre]     # TAG names should always be lowercase
---

# MITRE
- ID : `T1136.001`
- Tactic : `Persistence`
- Platforms: `Linux`, `Windows`, `macOS` 

# Local Account
Local accounts are those configured by an organization for use by users, remote support, services, or for administration on a single system or service.Adversaries may create a local account to maintain access to victim systems.

## Local Account On Windows

### command prompt

***Creating local user account using command prompt***

syntax
> `net user [username] [password] /add`

```batch
net user apt password123 /add
```

![localuser](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localuser.png)

***Create user account, then add user account to administrators group using command prompt***

syntax
> `net user [username] [password] /add`
> `net localgroup [group] [username] /add`

```batch
net user apt password123 /add

net localgroup administrators apt /add

```

![localuseradmin](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localuseradmin.png)

### powershell

***Creating local user account using powershell***

syntax
> `New-LocalUser -Name [username]`

```powershell
New-LocalUser -Name apt

```
![localuseradmin](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localuserpowershell.png)

## Local Account On Linux

***Creating local user account***

syntax

> `useradd -M -N -r -s /bin/bash [username]`

```bash
useradd -M -N -r -s /bin/bash apt

```
![localuserlinux](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localuserlinux.png)


# Mitigations
- Use multi-factor authentication for user and privileged accounts.
- Limit the usage of local administrator accounts to be used for day-to-day operations that may expose them to potential adversaries.

# References

- [https://attack.mitre.org/techniques/T1136/001/](https://attack.mitre.org/techniques/T1136/001/)
- [https://attack.mitre.org/mitigations/M1032/](https://attack.mitre.org/mitigations/M1032/)
- [https://attack.mitre.org/mitigations/M1026/](https://attack.mitre.org/mitigations/M1026/)
- [https://atomicredteam.io/persistence/T1136.001/](https://atomicredteam.io/persistence/T1136.001/)
