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

smb_login shows two users with valid password, but didn't have much luck using them.

Let's continue with the temp user.

found an xml file in the configs that contains, what appears to be an hash for the c.smith user.

<img width="927" alt="Screenshot 2020-02-05 at 15 00 50" src="https://user-images.githubusercontent.com/47299364/75966838-422f7580-5ecb-11ea-8f5f-ec370d14cfde.png">

<img width="956" alt="Screenshot 2020-02-05 at 15 01 06" src="https://user-images.githubusercontent.com/47299364/75966900-570c0900-5ecb-11ea-80b8-31fcd0608aa4.png">

There was also another file that contained the notepad++ history files, which reveals an interesting path:

<img width="688" alt="Screenshot 2020-02-05 at 15 04 06" src="https://user-images.githubusercontent.com/47299364/75967007-7440d780-5ecb-11ea-85b9-518b0e6f41ae.png">

Moving into IT/Carl from the locating listed in the notepad file i see a VB project folder:

<img width="931" alt="Screenshot 2020-02-25 at 09 10 11" src="https://user-images.githubusercontent.com/47299364/75967135-abaf8400-5ecb-11ea-9302-fa459972d82c.png">

After getting this project, running it in visual studio and providing the RU_config.xml that contained the hash for the c.smith it reveals the password the for c.smith user:

<img width="774" alt="Screenshot 2020-02-26 at 08 02 43" src="https://user-images.githubusercontent.com/47299364/75967202-c550cb80-5ecb-11ea-969d-4f7999354b90.png">

let's mount the users folder using this user to at least read the user flag:

<img width="927" alt="Screenshot 2020-02-26 at 08 23 17" src="https://user-images.githubusercontent.com/47299364/75967267-ddc0e600-5ecb-11ea-8800-7eed8450f3b9.png">

<img width="619" alt="Screenshot 2020-02-26 at 08 23 54" src="https://user-images.githubusercontent.com/47299364/75967291-e4e7f400-5ecb-11ea-8b82-c196ac7f9045.png">

user flag acquired.

the box had also a service listening on a higher port that was available through telnet but required a debug password to reveal more commands. below we can see a file called exactly that, debug mode password but seems empty.


<img width="857" alt="Screenshot 2020-02-26 at 08 26 00" src="https://user-images.githubusercontent.com/47299364/75967448-1e206400-5ecc-11ea-8067-fe402f43f5e0.png">

there's also an executable, let's get all of this as it might be usefull.

the debug file had data saved in ADS(alternate data stream) an attribute that belongs to NTFS file system that's why it was showing nothing in normal view of linux. SMB contains a special command to retrieve all the data;

<img width="1267" alt="Screenshot 2020-02-26 at 12 08 47" src="https://user-images.githubusercontent.com/47299364/75967576-5162f300-5ecc-11ea-8a12-08ec3ad34599.png">

and voila it reveals the password:

<img width="793" alt="Screenshot 2020-02-26 at 12 09 44" src="https://user-images.githubusercontent.com/47299364/75967620-6049a580-5ecc-11ea-909f-71a28980aa45.png">


using this password in the higher service works, and show where is tbis Hqk exe running:

<img width="780" alt="Screenshot 2020-02-26 at 12 11 19" src="https://user-images.githubusercontent.com/47299364/75967687-79eaed00-5ecc-11ea-888f-716ff2269d1c.png">

<img width="552" alt="Screenshot 2020-02-26 at 12 12 02" src="https://user-images.githubusercontent.com/47299364/75967714-82dbbe80-5ecc-11ea-8eec-70332842a77a.png">

this tool allow us to navigate through the file system, let's try to hunt for some usefull file.

there was a particular file, ldap.conf that contains the hash for administrator like the one i found for c.smith, so cracking should be done in same way using the same executable.

<img width="600" alt="Screenshot 2020-02-27 at 07 58 55" src="https://user-images.githubusercontent.com/47299364/75967883-c59d9680-5ecc-11ea-9557-a67932ee5866.png">

bam, we got the admin password:

<img width="790" alt="Screenshot 2020-02-27 at 08 17 54" src="https://user-images.githubusercontent.com/47299364/75967908-d0f0c200-5ecc-11ea-8878-cd47e45afad9.png">

