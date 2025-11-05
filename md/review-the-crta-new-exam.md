## Review: The New CRTA Exam — Is It Easier Now?

Evening. At 08:00 on October 25, 2025, my friend Rama took the CRTA (Certified Red Team Analyst) exam — it only cost $9 because it was on discount. I had previously purchased the same certification, which follows a format similar to OSCP: you must create a report and submit it via email to CyberWarfare, the provider and developer of the certification.

To be honest, the older CRTA was quite challenging — you had to perform a Golden Ticket attack to gain Enterprise Admin privileges and then read the secret.xml file. The new CRTA, however, is noticeably easier; there’s no pivoting or complex chaining involved.

So, what kinds of attacks can you expect in the new CRTA? Interestingly, the Active Directory setup no longer includes kerberoasting — which is quite unusual since Kerberos-based attacks are typically a core part of any AD assessment. Moreover, if you want to pass the exam, you need to pay close attention to usernames, especially on Windows machines, as they contain extremely valuable clues. You can actually obtain the secret.xml simply by analyzing the BloodHound results.

<b>Below are the attacks featured in the latest CRTA version — the ones you might want to study first: </b>

- Local File Inclusion (https://blog.nody.cc/posts/container-breakouts-part1/)
- Misconfig Sudo For Privilege Escalation (https://gtfobins.github.io/)
- DCSync Attack (https://iam0xc4t.medium.com/dcsync-attack-gaining-domain-admin-via-active-directory-replication-3280a759cd95)
- Pass The Hash (https://viperone.gitbook.io/pentest-everything/everything/everything-active-directory/lateral-movement/alternate-authentication-material/wip-pass-the-hash)
- Credential Dumping (https://wadcoms.github.io/)
- ACL Misconfig Abuse (https://www.thehacker.recipes/ad/movement/dacl/)

That’s pretty much all you need to study if you want to pass the CRTA exam. Unlike before, the new version is less challenging, and the report format is still engaging but much simpler. That’s it for this short, straightforward review — nothing complicated, just to the point.

Thanks for reading, and I hope this helps you pass your CRTA exam! See you! 👋
