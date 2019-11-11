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
