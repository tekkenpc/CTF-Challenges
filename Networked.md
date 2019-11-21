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

Under the page upload.php there's an upload box for images only, this is interesting as we can manipulate a php reverse shell to actually looks like a GIF file.

<img width="597" alt="4" src="https://user-images.githubusercontent.com/47299364/69315784-a9385f80-0c37-11ea-9a4b-033cb3c998e4.png">

And that's exactly what i did, created the php reverse shell and included the magic bit for the GIF file as can be seen below:

<img width="865" alt="5" src="https://user-images.githubusercontent.com/47299364/69315878-e6045680-0c37-11ea-9027-4218a80c95c7.png">

Then i just need to setup a listener using netcat on my local machine in order to get the reverse shell:

<img width="316" alt="6" src="https://user-images.githubusercontent.com/47299364/69315920-fddbda80-0c37-11ea-8bb8-07bf6156dc03.png">

Once the file is uploaded and I navigate on the browser to the photos.php page, the shell get's established back to me:

<img width="892" alt="7" src="https://user-images.githubusercontent.com/47299364/69316009-282d9800-0c38-11ea-9bd2-0889501575f7.png">

Cool, got a shell as apache user!
