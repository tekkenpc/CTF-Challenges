Heist - Windows machine from HTB

Initial port scanning against the target:

root@kali:~# nmap -v -p 1-65535 -sV -O -sS -T4 10.10.10.149
