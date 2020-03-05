Sauna box from Hack the box

typical nmap scan for the 1st step:

<img width="561" alt="Screenshot 2020-02-27 at 09 29 45" src="https://user-images.githubusercontent.com/47299364/75969216-e535be80-5ece-11ea-8792-b641a9dd939d.png">

not much info revealed, smb, webserver(which normally don't happens in windows box).

enum4linux also don't reveal much, just the domain:

<img width="761" alt="Screenshot 2020-02-27 at 09 52 25" src="https://user-images.githubusercontent.com/47299364/75969305-08f90480-5ecf-11ea-8635-2c8ee93160f9.png">

looking into the website, we can see all the people that belongs to the company, so it was a guess game for the usernames.

<img width="1153" alt="Screenshot 2020-02-27 at 12 56 30" src="https://user-images.githubusercontent.com/47299364/75969389-2fb73b00-5ecf-11ea-92fe-fdb92a779ef7.png">

tried all combinations i could think of and it worked for fsmith to get it's TGT ticket:

<img width="1404" alt="Screenshot 2020-02-27 at 12 56 03" src="https://user-images.githubusercontent.com/47299364/75969430-3ba2fd00-5ecf-11ea-9a96-eeee288ac8ec.png">

cracking it with john gives me fsmith password:

<img width="926" alt="Screenshot 2020-02-27 at 12 56 44" src="https://user-images.githubusercontent.com/47299364/75969478-4f4e6380-5ecf-11ea-8c70-096ef4ab8009.png">

let's use evil-winrm to login to the box and get user flag:

<img width="797" alt="Screenshot 2020-02-27 at 13 00 33" src="https://user-images.githubusercontent.com/47299364/75969529-5e351600-5ecf-11ea-9179-d23f30c09ba5.png">

<img width="731" alt="Screenshot 2020-02-27 at 13 00 44" src="https://user-images.githubusercontent.com/47299364/75969552-6725e780-5ecf-11ea-9297-52bc7e50416e.png">

also tried to use a new tool, powercat.ps1 with is a utility to create reverse shells:

<img width="731" alt="Screenshot 2020-02-28 at 10 06 42" src="https://user-images.githubusercontent.com/47299364/75969664-90df0e80-5ecf-11ea-8fc3-5c5242baa5ec.png">
<img width="1376" alt="Screenshot 2020-02-28 at 10 06 20" src="https://user-images.githubusercontent.com/47299364/75969634-89b80080-5ecf-11ea-8126-fc9c46524e96.png">
<img width="989" alt="Screenshot 2020-02-28 at 10 06 53" src="https://user-images.githubusercontent.com/47299364/75969693-9a687680-5ecf-11ea-9aa6-c8be6538945e.png">

this was just an experiment, nothign useful was added.

tried a simple command to enumerate the registry, which is to hunt for password.

<img width="979" alt="Screenshot 2020-03-03 at 17 58 13" src="https://user-images.githubusercontent.com/47299364/75969796-bd932600-5ecf-11ea-836e-0f7cf278b501.png">

and it worked, got a password:

<img width="576" alt="Screenshot 2020-03-03 at 17 57 34" src="https://user-images.githubusercontent.com/47299364/75969816-c683f780-5ecf-11ea-8edd-d90d1141f1a6.png">

forgot to take the screenshot of listing the users folder. there i found another account, svc_loanmgr and this is where i'm going to try this password.

<img width="890" alt="Screenshot 2020-03-03 at 17 59 31" src="https://user-images.githubusercontent.com/47299364/75969937-f29f7880-5ecf-11ea-9ae2-33545919f13e.png">

found a new tool, winPEAS.exe that you can run on the box to reveal also vulnerabilities that will lead to priv escalation

<img width="1185" alt="Screenshot 2020-03-03 at 21 07 32" src="https://user-images.githubusercontent.com/47299364/75970020-0f3bb080-5ed0-11ea-986f-a966a4594a6a.png">
<img width="1304" alt="Screenshot 2020-03-03 at 21 07 46" src="https://user-images.githubusercontent.com/47299364/75970048-182c8200-5ed0-11ea-8d92-2f3b01a05882.png">

let's try to use the secretsdump.py from the impacket suite to get some hashes:

<img width="926" alt="Screenshot 2020-03-03 at 21 14 35" src="https://user-images.githubusercontent.com/47299364/75970111-2da1ac00-5ed0-11ea-9490-2718f6e161ce.png">

sweet we got them, let's now use psexec with passthehash technique to login:

<img width="1379" alt="Screenshot 2020-03-03 at 21 32 07" src="https://user-images.githubusercontent.com/47299364/75970176-44e09980-5ed0-11ea-987c-60534a46bc56.png">

<img width="959" alt="Screenshot 2020-03-03 at 21 33 06" src="https://user-images.githubusercontent.com/47299364/75970196-4b6f1100-5ed0-11ea-93fc-f54ea5de78ea.png">

<img width="582" alt="Screenshot 2020-03-03 at 21 38 49" src="https://user-images.githubusercontent.com/47299364/75970238-56c23c80-5ed0-11ea-933c-b4887f285d22.png">



