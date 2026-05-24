Anonforce TryHackMe Writeup
===========================




by [BenMosh](https://medium.com/@guybmontop?source=post_page---byline--17a7e5266b69---------------------------------------)




by BenMosh
----------

I started by searching open ports on the IP

![Nmap scan](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*uyLmEGNNa3jErP6LF_Nypw.png)

I saw port 21 ftp is open and port 22 ssh is open.

I then cheeked if there the default user Anonymous on ftp, and to my lack it indeed was

![captionless image](https://miro.medium.com/v2/resize:fit:1360/format:webp/1*USf3F1S6G7yRXiRBxVHPQA.png)

Now that I got in I checked the /home directory to find user.txt, I found the user melodias home directory and in it there was user.txt

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*b3qwarfKvu4rJ5jXCO7e7A.png)

to get the file I used “get” to copy the file to my own machine.

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*DsIOkT0hP_I7SziSpYojJA.png)

and I got the first flag

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*rYfT_lng6TlD74TZGawPpw.png)

I then looked for any mode directories in the ftp and I founded “notread”

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*SyaTpa9lJorhggIroW6L1w.png)

If I go in it if found two files “backup.pgp” and “private.asc” and I also used “get” to copy the files to my machen

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*EM5XEQwmMg0uikcg7FlweQ.png)

Now I need to crack the hash in “private.asc” for that I used gpg2john

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*cOGYmHaCUsSgh1qAYwVWgQ.png)

and then I used john to get the password for backup.gpg

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*I1aSvWihNK-q43Vsgcoipw.png)

Now that we know that the password for backup.gpg is xbox360 we can dycrypt the file

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*TjUIiHHxLLGYgMzbXVUwmQ.png)

we get the hash of the password for the root user in ssh

we know that the password are hashed with sha-256 so I used hashcat to get the actual password

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*xpI5zUWxLzgpnSaMqte9RQ.png)

the password is “hikari” and now I can connect to the user root using ssh

and from this step its just connecting to the ssh and reading the file root.txt

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*jgPQaTsqXy2ZkdDQdqoO6Q.png)
