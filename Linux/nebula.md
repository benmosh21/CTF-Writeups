<img width="485" height="188" alt="image" src="https://github.com/user-attachments/assets/213073c4-ea0a-4a8d-987a-3c1849fe7506" />i started by doing the command 'ls -lsaR / | grep "level00" -b10'
this will find all files that have level00
then i got the /rofs/bin/.../flag00

<img width="670" height="190" alt="image" src="https://github.com/user-attachments/assets/116ecb02-ec95-49f3-998a-da23b30f41be" />

so i finished level00

level01
i saw that the file use /usr/bin/env and this is just my path, so if i set my path to a directory with my files
i made a file called echo to change the echo used in the file. to run the getflag command

<img width="485" height="188" alt="image" src="https://github.com/user-attachments/assets/8bceb5d4-407a-49ff-8ce5-a221002a0cf3" />

then i change my path and run it. this way it worked

<img width="674" height="88" alt="image" src="https://github.com/user-attachments/assets/e96f285a-282f-4c4b-bc1e-e924c1c38db4" />

level02

i saw that it echo the USER env var, if i can change it i can controll everything
so if i do export USER="; getflag;" i put the getflag in the middle of the code.

<img width="693" height="147" alt="image" src="https://github.com/user-attachments/assets/c50f8f38-dec7-4639-93d6-feca1ecf8a30" />

level03

on the /home/flag03 there is directory and a file, the file 

<img width="500" height="216" alt="image" src="https://github.com/user-attachments/assets/f8be3cd7-d7dd-413a-b952-30edaf4e1675" />

this code do is running script files from writable.d/
so i made a file that return the getflag into a file:
getflag > /tmp/flag03.out
and it worked

<img width="656" height="72" alt="image" src="https://github.com/user-attachments/assets/b01f8711-c9b7-4267-b5b5-8b6b9c38471a" />

level04
there is a code that wont let me read token.
insted of giving it the token i gave it a symlink to the token file.

<img width="770" height="78" alt="image" src="https://github.com/user-attachments/assets/ce4a913b-a443-467b-b897-cd86107eebde" />

after that i ussed the token as the password

<img width="707" height="121" alt="image" src="https://github.com/user-attachments/assets/bcbf6114-68d7-4d26-ad29-decd842aa0b3" />


level05:
inside /home/flag05 there is the .backup that i have r-x as others, that mean i can copy the files from it.

<img width="740" height="306" alt="image" src="https://github.com/user-attachments/assets/91360c9b-fbc8-4a7a-9319-538cd460c978" />

inside of it there is a tar so i moved it to my home directory, after unpacking it it gave me .ssh
so i need to conenct to the flag05 using the .ssh keys
i gave perms to the .ssh 

<img width="908" height="212" alt="image" src="https://github.com/user-attachments/assets/bd0869bb-fccd-4518-8e5d-3eb7020371b9" />

<img width="676" height="80" alt="image" src="https://github.com/user-attachments/assets/f1010f21-7980-42f5-8bab-a471c6a1ecab" />




