Monteverde machine from Hack the box

typical nmap scan:

<img width="661" alt="Screenshot 2020-03-04 at 09 26 27" src="https://user-images.githubusercontent.com/47299364/75970366-82ddbd80-5ed0-11ea-8888-7e888b31e77a.png">

running enum4linux reveals a lot of info, users, groups, etc.

<img width="1393" alt="Screenshot 2020-03-04 at 09 29 08" src="https://user-images.githubusercontent.com/47299364/75970437-9d179b80-5ed0-11ea-9bfc-c9b287c7840c.png">

<img width="940" alt="Screenshot 2020-03-04 at 09 29 25" src="https://user-images.githubusercontent.com/47299364/75970459-a86ac700-5ed0-11ea-9079-caa7b09dd2bd.png">

<img width="913" alt="Screenshot 2020-03-04 at 09 29 48" src="https://user-images.githubusercontent.com/47299364/75970486-b0c30200-5ed0-11ea-8181-e29b924f365c.png">

after trying bruteforce and all type of combinations, i was able to login with the SABatchJobs with same username and password:

<img width="720" alt="Screenshot 2020-03-04 at 14 09 11" src="https://user-images.githubusercontent.com/47299364/75970608-e667eb00-5ed0-11ea-880b-64bb6fd03137.png">

let's try to smbmap with this user:

<img width="820" alt="Screenshot 2020-03-04 at 14 11 27" src="https://user-images.githubusercontent.com/47299364/75970625-ee278f80-5ed0-11ea-9374-198f7bb0c95b.png">

let's enumerate as much as possible with this user:

<img width="891" alt="Screenshot 2020-03-04 at 14 32 10" src="https://user-images.githubusercontent.com/47299364/75970723-12836c00-5ed1-11ea-8499-a0052f1692cf.png">

there's a password inside the azure.xml file:

<img width="782" alt="Screenshot 2020-03-04 at 14 33 08" src="https://user-images.githubusercontent.com/47299364/75970779-262ed280-5ed1-11ea-9522-55bbc8ff4a25.png">

let's try to use that credentials with mhope:

<img width="746" alt="Screenshot 2020-03-04 at 14 36 04" src="https://user-images.githubusercontent.com/47299364/75970837-3941a280-5ed1-11ea-8bca-c44aa32a8735.png">

used evil-winrm to login with mhope user:

<img width="973" alt="Screenshot 2020-03-04 at 14 39 34" src="https://user-images.githubusercontent.com/47299364/75970938-5fffd900-5ed1-11ea-9382-b151e1d22b03.png">

found a .Azure folder that belongs to this user:

<img width="1091" alt="Screenshot 2020-03-04 at 14 49 47" src="https://user-images.githubusercontent.com/47299364/75970988-6e4df500-5ed1-11ea-9cdf-d4371e214b03.png">

there's a bunch of files inside that means machine is connected with Azure somehow.

<img width="833" alt="Screenshot 2020-03-04 at 14 50 00" src="https://user-images.githubusercontent.com/47299364/75971042-7d34a780-5ed1-11ea-9989-6032d4ac6201.png">

there was a very recent exploit using azure ad sync to get admin credentials and that's what i used:
to run this i had to upload the AdDecrypt.exe and mcrypt.dll necessary to run the exploit:

<img width="1005" alt="Screenshot 2020-03-04 at 16 09 37" src="https://user-images.githubusercontent.com/47299364/75971267-d3a1e600-5ed1-11ea-8114-4bbb4cdff6cf.png">

<img width="925" alt="Screenshot 2020-03-04 at 16 09 22" src="https://user-images.githubusercontent.com/47299364/75971119-95a4c200-5ed1-11ea-89d6-1a91b00ca902.png">

<img width="1438" alt="Screenshot 2020-03-04 at 16 14 06" src="https://user-images.githubusercontent.com/47299364/75971313-e2889880-5ed1-11ea-9cd4-b230ebbd103d.png">

and that's it, very fun box too :)

