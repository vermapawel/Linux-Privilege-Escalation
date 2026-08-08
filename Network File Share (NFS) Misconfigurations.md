**Network File Share (NFS) Misconfigurations**

We can upload or download any file with the help of NFS

**cat /etc/exports** >> NFS configuration file.

<img width="786" height="289" alt="image" src="https://github.com/user-attachments/assets/f31d0ff4-5036-49cc-84e4-ecda23509867" />

<img width="786" height="202" alt="image" src="https://github.com/user-attachments/assets/7258d7a0-8ee7-41f5-a30a-70b748cda7a6" />

**/tmp *(rw,sync,insecure,no_root_squash,no_subtree_check)**

/tmp folder is shared. It has read and write permissions.

**root_squash** is a protection on NFS. If anyone mount the /tmp folder, and he is a root user in his local machine, and he write a file in /tmp folder, so the owner of that file will not be a root user. It could be any random user if root_squash protection is set.

Now if it **no_root_squash** it means this protection is off and if a root user will create any file on the mounted folder, the owner of that will be root in target machine as well.

<img width="786" height="378" alt="image" src="https://github.com/user-attachments/assets/9ebc9536-54a6-41d4-96af-d1e919b0b96b" />

NFS is enabled on the target machine on port 2049.

NFS use help of rpcbind protocol to make connection between server and client. So most of the time, if NFS is enabled, rpcbind is also enabled and vice-versa.

**showmount** >> It will tell what all things are shared with NFS.

<img width="786" height="149" alt="image" src="https://github.com/user-attachments/assets/5fb1b61f-e32a-48f8-9b70-a610d0b00b81" />

/tmp folder is shared.

Now, if we want to see what content are in /tmp folder, we need to mount it in our machine.

For mounting we have a default folder in linux /mnt. We need to create a folder in /mnt and there we can mount the folder.

***mount -o rw,vers=3 10.49.144.201:/tmp /mnt/tmp***

-o rw → Mount the share with read-write permissions.

vers=3 → Use NFS version 3 for the mount.

10.49.144.201:/tmp → The NFS server and exported directory.

/mnt/tmp → Local directory where the share will be mounted.

<img width="786" height="298" alt="image" src="https://github.com/user-attachments/assets/142c0080-8dee-4832-a134-7dcf29b919da" />

Target machine’s /tmp folder is mounted. We have the same content that are in target machine /tmp folder.

Now, I am a root user in our local machine and I have mounted the shared folder. Also I have write permissions on this folder.

Lets create a txt file in the mounted folder.

<img width="786" height="321" alt="image" src="https://github.com/user-attachments/assets/8132c690-38f2-4acc-a301-275d31914311" />

The owner and group of this file.txt is root.

<img width="786" height="239" alt="image" src="https://github.com/user-attachments/assets/a0294ddb-f8b3-4deb-b1f4-a70ad8961c7c" />

We have that same txt file in target machine as well and owner of this file is root.

Now lets copy /bin/bash in the mounted folder.

<img width="786" height="476" alt="image" src="https://github.com/user-attachments/assets/9e193859-9a43-490b-a15c-1074993345ae" />

We have given SUID permission to it.

Lets check on the target machine

<img width="786" height="233" alt="image" src="https://github.com/user-attachments/assets/b3015d25-a22c-4de2-94df-d6112c4c319d" />

We have the bash file. Owner of the file is root.

Lets try to execute the bash SUID

<img width="786" height="202" alt="image" src="https://github.com/user-attachments/assets/2548c05a-4326-4288-8f4a-1616c8d22edf" />

Here we are getting some error regarding some library. As this is a very old machine it seems that some libraries are not supported.

Lets create a payload with the help of msfvenom.

***msfvenom -p linux/x64/exec CMD=”/bin/sh” -f elf -o shell***

msfvenom → Tool to generate payloads for exploitation.

-p linux/x64/exec → Payload type: execute a command on a Linux x64 system. Here we dont need a nc listener in our machine. It will execute.

CMD="/bin/sh" → The command that will run when the payload executes (in this case, it spawns a shell).

-f elf → Output format: ELF binary (Linux executable).

-o shell → Output file name: shell.

<img width="786" height="327" alt="image" src="https://github.com/user-attachments/assets/798fab39-5a38-41e6-bed9-c62f782d1bee" />

Lets make it SUID

<img width="786" height="252" alt="image" src="https://github.com/user-attachments/assets/9a1698dd-66dd-4d8e-b05e-486566592f83" />

On the target machine

<img width="786" height="249" alt="image" src="https://github.com/user-attachments/assets/bb221a94-0056-403d-978b-e9cc33da1441" />

Lets execute this shell

<img width="786" height="212" alt="image" src="https://github.com/user-attachments/assets/4fdc9545-8906-4b13-9ba6-059b02cab238" />

We are root user !!!

***umount /mnt/tmp*** > To unmount the folder.
