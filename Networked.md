Networked machine from HTB

Starting with port scanning:

root@kali:~# nmap -v -p 1-65535 -sV -O -sS -T4 10.10.10.146


<img width="1393" alt="Screenshot 2019-11-13 at 00 20 42" src="https://user-images.githubusercontent.com/47299364/68719102-747b3700-05ab-11ea-96fc-33368139b1eb.png">

nmap show ssh and http ports open.

checking http webpage:

<img width="492" alt="Screenshot 2019-11-13 at 00 18 37" src="https://user-images.githubusercontent.com/47299364/68719014-2e25d800-05ab-11ea-95e6-e39c0733c445.png">

no hidden things in the source code. No robots.txt page available.

Let's fire up dirbuster to look for folder or other hidden pages.

Dirbuster reveals some interesting paths and even a backup tar file:

<img width="580" alt="Screenshot 2019-11-13 at 00 29 38" src="https://user-images.githubusercontent.com/47299364/68719594-c07aab80-05ac-11ea-8ff1-6e16b396910c.png">

