
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
