Postman from HTB

Starting with nmap scanning:

<img width="904" alt="Screenshot 2019-11-11 at 23 34 24" src="https://user-images.githubusercontent.com/47299364/68626533-d5d3d500-04db-11ea-8e7e-498308f1450b.png">

After performing the nmap scan we identify the regular ports ssh 22, http 80 and an interesting high port tcp 10000.
Quick internet search reveals that this port belongs to web application Webmin. Cool!

The website reveals a regular page:

<img width="1216" alt="Screenshot 2019-11-11 at 23 37 19" src="https://user-images.githubusercontent.com/47299364/68626704-3ebb4d00-04dc-11ea-961b-c05cb84dc5c0.png">


I've ran nikto and dirb to look for some hidden folders, or interesting ones, nothing was properly revealed.

Let's run a second port scan to see if we missed something:

indeed we missed port 6379, Redis server:

<img width="1367" alt="Screenshot 2019-11-11 at 23 46 41" src="https://user-images.githubusercontent.com/47299364/68627221-87bfd100-04dd-11ea-8463-014dde71bab6.png">

Got redis-cli installed on my machine to see if it helped revealing something. Nothing usefull came up apart from listing client session, etc.

After looking around for exploits, found one in github (https://github.com/Avinash-acid/Redis-Server-Exploit) about misconfigured redis server. 

Basically the script only needs to destination and a username, common user on redis installation is redis:

<img width="1367" alt="Screenshot 2019-11-11 at 23 51 30" src="https://user-images.githubusercontent.com/47299364/68627510-382dd500-04de-11ea-828d-1a3ee3764119.png">

And we're in as redis user:

<img width="1367" alt="Screenshot 2019-11-11 at 23 54 06" src="https://user-images.githubusercontent.com/47299364/68627650-9490f480-04de-11ea-86d9-aeb717ed1c97.png">


We can see that there's is a main user, Matt and it contains the user flag in the home directory. Unfortunately user redis don't have permissions to read it:

<img width="1367" alt="Screenshot 2019-11-11 at 23 55 44" src="https://user-images.githubusercontent.com/47299364/68627748-e5a0e880-04de-11ea-93e7-07d106ec8e6b.png">

After a couple time enumerating the machine, there was an interesting file present in /opt/ folder. An ssh private key for authentication of the user Matt.

<img width="759" alt="Screenshot 2019-11-11 at 23 58 16" src="https://user-images.githubusercontent.com/47299364/68627856-27319380-04df-11ea-8778-822a5e832d3b.png">

Let's try to crack this out.

To crack this private key i will be using john with rockyou list available in kali.

john requires a specific format to crack it. To have it in this expected format i've used the tool ssh2john.py

<img width="1371" alt="Screenshot 2019-11-12 at 00 00 32" src="https://user-images.githubusercontent.com/47299364/68627968-78da1e00-04df-11ea-80bd-0e7f384fca5e.png">

After sending this to a file(key.bak) i've finally fired up john to crack this one:

<img width="899" alt="Screenshot 2019-11-12 at 00 02 36" src="https://user-images.githubusercontent.com/47299364/68628059-c35b9a80-04df-11ea-9931-48060a12c641.png">

Passphrase cracked: computer2008

Tried several attempts to connect to the box 10.10.10.160 using the private key, although ssh is blocked for this user.


<img width="776" alt="Screenshot 2019-11-12 at 00 04 04" src="https://user-images.githubusercontent.com/47299364/68628138-f7cf5680-04df-11ea-9651-f092a3d785b8.png">

That's when came to my mind that i didn't explore the port 10000 we found earlier yet. Let's have a look:

Hum, webmin authentication portal:

<img width="1124" alt="Screenshot 2019-11-12 at 00 05 04" src="https://user-images.githubusercontent.com/47299364/68628185-1a616f80-04e0-11ea-8d65-64f6c17c885d.png">

Let's give a try with user Matt and password computer2008

We're in as Matt user:

<img width="1371" alt="Screenshot 2019-11-12 at 00 07 23" src="https://user-images.githubusercontent.com/47299364/68628294-71ffdb00-04e0-11ea-81f7-89d2fe27f432.png">

Let's try to look for known exploits for Webmin up to version 1.910

This one looks interessing, i can see the user can execute package updates:

<img width="1282" alt="Screenshot 2019-11-12 at 00 09 21" src="https://user-images.githubusercontent.com/47299364/68628386-bd19ee00-04e0-11ea-9a6b-7ce1778d5f67.png">

<img width="1090" alt="Screenshot 2019-11-12 at 00 10 12" src="https://user-images.githubusercontent.com/47299364/68628412-d15deb00-04e0-11ea-8bdc-75fc38e8f8a3.png">

Ok, let's give it a try. Firing up metasploit console: 

<img width="1374" alt="Screenshot 2019-11-12 at 00 11 09" src="https://user-images.githubusercontent.com/47299364/68628468-f4889a80-04e0-11ea-8747-95183d42f53d.png">

Let's choose the exploit number 2:

Checking the options, to make sure we have everything and we do have everything:

<img width="986" alt="Screenshot 2019-11-12 at 00 12 23" src="https://user-images.githubusercontent.com/47299364/68628538-2c8fdd80-04e1-11ea-8187-2797ab792583.png">

All set accordingly:

<img width="969" alt="Screenshot 2019-11-12 at 00 13 55" src="https://user-images.githubusercontent.com/47299364/68628589-55b06e00-04e1-11ea-9118-8cefb826a525.png">


Weird error:

<img width="806" alt="Screenshot 2019-11-12 at 00 14 29" src="https://user-images.githubusercontent.com/47299364/68628633-6eb91f00-04e1-11ea-900d-11292facff6f.png">

Oh wait, webserver is running under https and the ssl flag was set to false. After changing it to true:

<img width="982" alt="Screenshot 2019-11-12 at 00 15 37" src="https://user-images.githubusercontent.com/47299364/68628671-91e3ce80-04e1-11ea-9ed7-b9d4d506711c.png">

yes, the exploit works and we get a shell:

<img width="1111" alt="Screenshot 2019-11-12 at 00 16 13" src="https://user-images.githubusercontent.com/47299364/68628699-b8a20500-04e1-11ea-8ee4-78b92c2786f1.png">

And a shell as root already, what a lucky one!!!!


<img width="1047" alt="Screenshot 2019-11-12 at 00 17 51" src="https://user-images.githubusercontent.com/47299364/68628771-e2f3c280-04e1-11ea-9674-6d09deeaecd5.png">

Let's go find out the flags:

User flag:
<img width="475" alt="Screenshot 2019-11-12 at 00 19 05" src="https://user-images.githubusercontent.com/47299364/68628843-13d3f780-04e2-11ea-96b4-95f603183297.png">


Root flag:
<img width="696" alt="Screenshot 2019-11-12 at 00 20 46" src="https://user-images.githubusercontent.com/47299364/68628911-4bdb3a80-04e2-11ea-8e6a-72d74a1f0aba.png">
