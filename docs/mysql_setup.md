### MySQL installation
When you finish apt-get update and upgrade it is time to install MySQL.
```bash
sudo apt-get install mysql-server python-mysqldb mysql-client
```
During the installation you should be prompted to set root password and etc. If you have not receive this notification in the terminal paste. It is recommended to turn off remote access for root and delete test databases. After this you need also to reload privileges.
```bash
mysql_secure_installation
```
To access MySQL db simply write in terminal
```bash
mysql -u root -p
```
and next type the root password that you set in previous step. Now you are in the MySQL shell (probably MariaDB). Now let's break into it.


### phpMyAdmin
After setting MySQL to access and manage it with nice UI, it is recommended to install phpmyadmin. To do it please type in the terminal
```bash
sudo apt-get install phpmyadmin
```
During the installation you will be prompted to chose between apache and light-httpd. Chose apache. Next you will be asked to configure db for phpMyAdmin, chose yes. Next screen will ask you to type root password to you MySQL and confirm it once again.
