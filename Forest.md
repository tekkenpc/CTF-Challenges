Forest windows machine from HTB

Starting with the typical nmap port scanning:

<img width="796" alt="1" src="https://user-images.githubusercontent.com/47299364/70398759-f9c50080-1a1e-11ea-92be-15633971565d.png">

<img width="1206" alt="2" src="https://user-images.githubusercontent.com/47299364/70398762-00537800-1a1f-11ea-9d00-ec683e2e34a2.png">

The port scanning shows a bunch of ports opened. Ldap, SMB, etc.

Let's try to enumerate some users using rpcclient:

<img width="512" alt="3" src="https://user-images.githubusercontent.com/47299364/70398775-2bd66280-1a1f-11ea-8152-9eb0b3c2d262.png">

<img width="1384" alt="4" src="https://user-images.githubusercontent.com/47299364/70398777-38f35180-1a1f-11ea-90a9-b713be11fe2e.png">

Some interesting users, after several attempts the only interesting one was svc-alfresco that gave us a TGT ticket.

<img width="1403" alt="5" src="https://user-images.githubusercontent.com/47299364/70398786-4f99a880-1a1f-11ea-872b-cacc121bb9f2.png">

Let's crack it using john:

<img width="883" alt="6" src="https://user-images.githubusercontent.com/47299364/70398837-722bc180-1a1f-11ea-8832-d3e1b86d8722.png">

