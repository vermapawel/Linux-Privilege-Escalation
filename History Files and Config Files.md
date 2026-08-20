**History File**

<img width="720" height="372" alt="image" src="https://github.com/user-attachments/assets/9a9d1381-8584-41d5-850e-0386358b2d04" />

Here we have multiple history files. We should check each of them. There could be some hidden credentials.

**-rw — — — — 1 user user 155 Jan 14 17:42 .bash_history**

The owner of this file is user and we are user. We have read and write permissions on it.

<img width="720" height="207" alt="image" src="https://github.com/user-attachments/assets/7d9ebcdb-f389-4940-ae07-d9d55f027b29" />

There is a command of mysql and username is root, password is password123. This credentials are for mysql.

However, let's try this password for root user.

<img width="681" height="193" alt="image" src="https://github.com/user-attachments/assets/ba38ca02-ea23-4df6-92ab-a635a55c9627" />

And we are root user.

===================================================================================

**Config files**

There must be a config file of the website like config.php and there may be some credentials written over there.

Any website uses database to store the data. To login that database there must be some credentials. Most of the time those credentials are stored in config files.

We can get these credentials by reading config files. Either those credentials are for some other users or they are for database credentials.

Whenever we logged in as www-data user, always read config files of the web. Most of the times we will get some credentials.

Lets read some config files.

<img width="720" height="509" alt="image" src="https://github.com/user-attachments/assets/9c7c07bf-4ac3-425c-8b90-973c700ab90d" />

Authentication user password is stored in /etc/openvpn/auth.txt

<img width="720" height="196" alt="image" src="https://github.com/user-attachments/assets/5e23e36d-00f3-48a7-81f5-a9f51709b830" />

Owner of this file is root however we have read permissions on it. We got the root password.
