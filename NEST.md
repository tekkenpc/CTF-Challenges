Nest box from Hack the box

starting with typical nmap scan:

<img width="647" alt="Screenshot 2020-02-05 at 10 25 27" src="https://user-images.githubusercontent.com/47299364/75966320-6cccfe80-5eca-11ea-8c9b-6c3f20e4dacb.png">

WE can see smb, let's try to enumrate the shares using metasploit:

<img width="703" alt="Screenshot 2020-02-05 at 10 25 38" src="https://user-images.githubusercontent.com/47299364/75966413-99811600-5eca-11ea-8a88-59fc61a82d73.png">

WE have some shares, let's try to enumerate them as anonymous as to mount on my machine to easily navigate through the files:

<img width="794" alt="Screenshot 2020-02-05 at 10 49 23" src="https://user-images.githubusercontent.com/47299364/75966445-ad2c7c80-5eca-11ea-80be-72a6e3f4717c.png">

Cool, one of the email is the welcome email that contains TempUser and password, let's try to use this account to get more info:

<img width="832" alt="Screenshot 2020-02-05 at 10 49 47" src="https://user-images.githubusercontent.com/47299364/75966537-d3521c80-5eca-11ea-88e7-8e5870308c48.png">

We can see more stuff with this user, let's start digging:

<img width="868" alt="Screenshot 2020-02-05 at 11 20 12" src="https://user-images.githubusercontent.com/47299364/75966576-de0cb180-5eca-11ea-9408-dcb944f303a6.png">

Or, let's try the initial credentials in another user(maybe they are dumb and didn't change it)

<img width="746" alt="Screenshot 2020-02-05 at 11 26 36" src="https://user-images.githubusercontent.com/47299364/75966653-f977bc80-5eca-11ea-9f87-958f4b75643f.png">



