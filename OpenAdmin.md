OpenAdmin box from Hack the box

Starting with the typical nmap scan:

<img width="815" alt="Screenshot 2020-01-14 at 10 21 37" src="https://user-images.githubusercontent.com/47299364/75962321-d72e7080-5ec3-11ea-9e4b-89d435735c87.png">

It reveals a web server running:

<img width="959" alt="Screenshot 2020-01-14 at 10 21 56" src="https://user-images.githubusercontent.com/47299364/75962609-60de3e00-5ec4-11ea-8312-3eed0b3aa225.png">

Searching for exploits affecting this version of openadmin, we see that there's is indeed a RCE for this version:

<img width="1285" alt="Screenshot 2020-01-14 at 10 23 28" src="https://user-images.githubusercontent.com/47299364/75965470-288d2e80-5ec9-11ea-9f36-39cd5a71beeb.png">

After running it we get the first shell in the system as www-data:

<img width="553" alt="Screenshot 2020-01-14 at 10 47 10" src="https://user-images.githubusercontent.com/47299364/75965526-39d63b00-5ec9-11ea-991a-10e8072cd6ec.png">

digging through the filesystem i found out the below file that contains credentials:

<img width="735" alt="Screenshot 2020-02-04 at 16 00 14" src="https://user-images.githubusercontent.com/47299364/75965842-afdaa200-5ec9-11ea-91c8-6004ab6b6723.png">

looks like something that worth a try, and after that i was able to login as jimmy:

<img width="664" alt="Screenshot 2020-02-04 at 17 25 12" src="https://user-images.githubusercontent.com/47299364/75965900-c3860880-5ec9-11ea-8187-c68ec377c864.png">

there was also a php page that called from the localhost revealed a ssh private key:

<img width="658" alt="Screenshot 2020-02-04 at 17 25 27" src="https://user-images.githubusercontent.com/47299364/75965965-def11380-5ec9-11ea-940f-489951758fa3.png">

let's try to crack it:

<img width="700" alt="Screenshot 2020-02-04 at 17 25 44" src="https://user-images.githubusercontent.com/47299364/75965996-eadcd580-5ec9-11ea-8b4c-fdf2b25c41d5.png">

bingo, password cracked, bloodninjas

let's login as joanna as this credentials was for her.

<img width="842" alt="Screenshot 2020-02-04 at 17 26 38" src="https://user-images.githubusercontent.com/47299364/75966081-0f38b200-5eca-11ea-8135-ee56b9611594.png">
 
as seen above, user executes nano with sudo privileges. so looks like a GTFObins exploit here :)

after giving the right GTFO inside the nano cmd we get a shell as root :)

<img width="877" alt="Screenshot 2020-02-04 at 17 35 04" src="https://user-images.githubusercontent.com/47299364/75966198-3ee7ba00-5eca-11ea-8a66-ba4e29aee3e8.png">
