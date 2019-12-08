Traverxec - Windows machine from HTB

Starting with the regular nmap port scan:

![1](https://user-images.githubusercontent.com/47299364/70398579-40b1f680-1a1d-11ea-894d-024825fa9d63.png)

Then I realized that there is a web server running on port 80 so let's check it out:

![2](https://user-images.githubusercontent.com/47299364/70398599-72c35880-1a1d-11ea-8be8-80402e094119.png)

According to the page it's a nostromo web server and there's a public exploit for that:

![3](https://user-images.githubusercontent.com/47299364/70398609-8ff82700-1a1d-11ea-8574-9e153f284377.png)

Exploiting that using metasploit I can get a shell as www-data in the server:

![4](https://user-images.githubusercontent.com/47299364/70398613-9be3e900-1a1d-11ea-8d4e-0d09b4a0571d.png)

![5](https://user-images.githubusercontent.com/47299364/70398624-b322d680-1a1d-11ea-8647-d31d9ef63665.png)

Looking into the /dev/shm/ there's david(the user) ssh private key:

![6](https://user-images.githubusercontent.com/47299364/70398629-c9309700-1a1d-11ea-93d1-a7de89da7db3.png)

![7](https://user-images.githubusercontent.com/47299364/70398637-e1081b00-1a1d-11ea-81ae-0a9130bbda4e.png)

It's just a matter of getting it and trying to crack it using john:

![8](https://user-images.githubusercontent.com/47299364/70398649-f5e4ae80-1a1d-11ea-90f6-fc1d95b6b880.png)
