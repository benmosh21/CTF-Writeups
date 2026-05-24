IDF BSMCH RIDDLE
================




by [BenMosh](https://medium.com/@guybmontop?source=post_page---byline--7735d635e7c6---------------------------------------)




we start the pazzle with a riddle:

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*5vGIhSOGApTXVqjgHQgsAg.png)

the goal is to decrypt the string in the red box

before I tried to solve the riddle I tried to analyze the strings, I use a few decryption methods and I found that base 92 works

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*_WyvKGtBSyFygpV0QdYWAQ.png)

Now we have a link to a website

In the website we see

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Um0AmGEc4qvwi2Ow4NcADw.png)

and after translating i got

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*B_1MGCVdT-A5jVJH-NfmQw.png)

It doesn’t help me at the moment to I will put it aside and continue with the puzzle

so i looked at the sorce code of the website

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*WiFVu4jj0oeLbX-OItzdtw.png)

and i saw a link to a image

![the green part is the image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*o0F__Er4LLNCQK7Ww_HHgg.png)

The image itself didnt give me any clue to how to continue so I thought it can be steganography

so I doawnloded the image to my kali virtual machine and tryied to find any hidden clues.

I tryed to look at the metadata of the file

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*u25Q_WPvhBEOrZulHf6U6w.png)

It didnt give me anything special

Then I tryed to use the command “binwalk” to find any hidden files inside the image

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*WIA8snXcKXOEjmDoLAacRg.png)

and I saw there are Zlib file PE file and XML file

so I extracted them

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*NsFi8cgWoroDAxMX471uvg.png)

Now I got 2 files “5B” and “5B.zlib”, I checked what type of files are them

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*EpSxqqtI85KMlVwtCQI38g.png)

after looking at “5B” I realised that it wasn’t an important file

Then I tried to decompress the file “5B.zlib”

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Hkft_K3l0K54meuPdwBObw.png)

It gave me the file “5B.raw” but it was also empty

So I gave a look to the result from the command binwalk i run ealier

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*XMt6r2iVxO3LpLJSyfmJ1w.png)

those are the offset of the files

If I use the commands to get the file

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*f2xre6zS7y394usY_i73TA.png)

I then unpacked the exe file “payload.exe”

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*6x5CohoZWz1VA_CfzI5rBQ.png)

and when i used “string” on the file I found this part

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*W3KG7I-uNQjWwQSwFA5A6Q.png)

which tell me to use rot43 on the “H9:E6=:@?]3D>49]:57]:=”

and i got “[whitelion.bsmch.idf.il](http://whitelion.bsmch.idf.il)”

This is what you see in the website

![captionless image](https://miro.medium.com/v2/resize:fit:814/format:webp/1*emOK0et6dyOTKX0CKpDtlQ.png)

when iI looked at the sorce code of the page I saw a link to a image

![captionless image](https://miro.medium.com/v2/resize:fit:916/format:webp/1*qpgculohg2D0kFfXYvYmSQ.png)

here is the “logo.ico”

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*I2ROAatuOV7TKGnzfBUwpA.png)

I immediately thought of using steganography to find hidden clues in the picture

I first tried binwalk but it didn’t give me anything useful

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*3G1Q2XrtQ7xUiufGuAQosw.png)

then I tryied exiftool to see the metadata but it also wasnt any help

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*WXDUV07-5KNinwnbZTo7nA.png)

after a while of looking i tried to see robots.txt and finally I got

![captionless image](https://miro.medium.com/v2/resize:fit:864/format:webp/1*n-P_aURnw2egM5mKVJYj2g.png)

Whivh redirect me to the

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*EifHxsAjkrhmLRcpxUX1eQ.png)

I try to do sql injection with username: “asd” password: “asd’ or 1=1 — “ and it got me in

This is what I see in the website

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*c4caAsL_zc33LDEyuRv7_Q.png)

If I press one of the green dots I get to ACCESS MISSION (each dot diffretn mission)

![captionless image](https://miro.medium.com/v2/resize:fit:1126/format:webp/1*iMfWqSk5Ua-jLO0hAZVvRg.png)

If I press it I get redirect to

![captionless image](https://miro.medium.com/v2/resize:fit:914/format:webp/1*MyfwIfGFE2a2Pd_JZ1S_BQ.png)

with id=1, and for each dot i get different id

I can use IDOR to accses the red dot mission but first lets look at the page

![id=1](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*-frAhk2sPFJSLRhduK55og.png)![id=2](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*1bDlDsTRmBR8HeAzCqmLTg.png)![id=3](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*LJdPAvzidSF8aOzDNbWdqQ.png)

these are the three MISSION I have access from the main page to get inside

If I change the “id” to “id=4” I get to the four MISSION

![id = 4](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*QLAn_M1lar6RL-W6yuCBMw.png)

Its tell me that I need to be admin
So there should be cookie of my role that I need to change

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*N8sjktNBDU3Iak43-OwIdw.png)

If I change that to role=admin I get

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*pC6n7SDxJcdvteDEijnzlA.png)

They use CVE-2025–20282 to attack our cisco devices

> CVE-2025–20282: A vulnerability in an internal API of Cisco ISE and Cisco ISE-PIC could allow an unauthenticated, remote attacker to upload arbitrary files to an affected device and then execute those files on the underlying operating system as root.

After a long time searching the website I found the flag
you add up all the {} we found in the website

there is {narniabale} from robots.txt {bsmch} from the home page {idf} from id=4 and {il} from id=4 after role=admin

and we got

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*aYB5OAeYXf2V3iSU_muSww.png)
