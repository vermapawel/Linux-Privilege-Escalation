There are two methods to take ssh shell.

**Method 1 : —**

Lets say we got a shell with the help of netcat. We know that SSH is running on the target machine but we dont have the password.

We can put our local machine public key in the target machine, we can connect with the target machine without password. And if we have access to root folder, we can login as root user.

<img width="565" height="139" alt="image" src="https://github.com/user-attachments/assets/3461acf8-2649-4aaa-a2dc-db909fb14e07" />

Here we dont have permissions to root folder.

We need to generate public and private keys for both target and local machine.

Lets create on target machine first.

***ssh-keygen -t rsa***

ssh-keygen → Tool to create SSH keys.

-t rsa → Specifies the key type: RSA (a widely used public-key cryptosystem).

<img width="720" height="515" alt="image" src="https://github.com/user-attachments/assets/d4081825-9559-41a9-bec5-5f87a89dd6af" />

Here we have put no passphrase. A passphrase is an additional layer of security for your private SSH key. It works like a password but is associated with the key file rather than your account. Even if someone steals your private key (id_rsa), they cannot use it without the passphrase. You’ll need to enter the passphrase whenever you use the key (or use an SSH agent to cache it).

<img width="720" height="236" alt="image" src="https://github.com/user-attachments/assets/4be28cbe-c8ba-4e0b-b7de-2d562e60e85f" />

private (id_rsa) and public (id_rsa.pub) keys are generated and stored in .ssh folder.

Now, lets create public and private keys on local machine.

***ssh-keygen -t rsa***

<img width="720" height="397" alt="image" src="https://github.com/user-attachments/assets/c6aca184-9c8a-4ed6-8ca3-a38400b9b0ab" />

<img width="720" height="137" alt="image" src="https://github.com/user-attachments/assets/28ce587a-6000-4cc9-ae91-ac3c00fd950f" />

Now copy the public key of out machine

<img width="720" height="142" alt="image" src="https://github.com/user-attachments/assets/aab251a2-5327-4077-af16-5dabd98b68c6" />

We will create a file called authorized_keys on the target machine.

authorized_keys contains public SSH keys of clients that are allowed to log in to that account without a password.

<img width="720" height="257" alt="image" src="https://github.com/user-attachments/assets/a20a859e-051b-4572-a7d2-469ae463c8ee" />

We have put the public key of the local mahine in the authorized_key folder. Now it will not ask for the password while doing ssh.

Lets try to login now via ssh

And we are able to login without any password.

**Method 2: —**

If we can read the private key of any user, we can login via ssh. It act like a password.

Lets go to the / folder of the target machine. There will be a folder .ssh

<img width="720" height="235" alt="image" src="https://github.com/user-attachments/assets/d85ada54-9031-4876-92c1-b9fc1f9ba888" />

The owner of the file is root but others have read access to root_key.

<img width="720" height="590" alt="image" src="https://github.com/user-attachments/assets/066d0313-6a2f-4246-a2e2-201c7d58558b" />

So, this is the private key of root user.

In out local machine lets create a file id_rsa and put the root private key there.

<img width="720" height="282" alt="image" src="https://github.com/user-attachments/assets/f270c552-858c-444c-a2f4-bc9037f90cb8" />

<img width="720" height="305" alt="image" src="https://github.com/user-attachments/assets/18e79c95-1271-4564-af24-6d65d12ae7ba" />

We have to give 600 permission to id_rsa file. The owner of the file has only read and write permission.

600 → Permission mode:

Owner: Read and write (rw-).

Group: No permissions (---).

Others: No permissions (---).

Now lets try to login as root user now using id_rsa

<img width="720" height="241" alt="image" src="https://github.com/user-attachments/assets/ec8aaf13-8a9f-4071-8d30-8e47f38cf8cf" />

And we are root user !!!!
