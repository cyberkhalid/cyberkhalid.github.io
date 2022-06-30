---
title: Local Account 
date: 2022-04-01 05:49:33 +0800
categories: [Red Teaming, Discovery]
tags: [redteam,linux, windows, mitre, local account, Discovery]     # TAG names should always be lowercase
---

# Local Account 
- ID : `T1087.001`
- Tactic : `Discovery`
- Platforms: `Windows`, `linux`, `macos`

# Local Account 

Adversaries may attempt to get a listing of local system accounts. This information can help adversaries determine which local accounts exist on a system to aid in follow-on behavior.

# Exploitations

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount1.png)

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount2.png)

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount3.png)

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount4.png)

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount5.png)

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount6.png)

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount7.png)

![localaccount](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/localaccount8.png)

# Mitigations

- Prevent administrator accounts from being enumerated when an application is elevating through UAC since it can lead to the disclosure of account names.

# References

- [https://attack.mitre.org/techniques/T1087/001/](https://attack.mitre.org/techniques/T1087/001/)
- [https://atomicredteam.io/discovery/T1087.001/](https://atomicredteam.io/discovery/T1087.001/)
