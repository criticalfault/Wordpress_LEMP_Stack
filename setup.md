#Wordpress on LEMP Installation Guide

1) Login into Digital Ocean Account in order to create a new Droplet.

2) Create a new Droplet with *Ubuntu 16.04.2 LTS x64*

3) Record the IP Address and custom password that arrives from Digital Ocean to prepare to SSH inside of the server once the droplet has been fully made.

4) Begin an SSH session with the root user using the password provided with the Digital Ocean Email.

5) Once logged into the root account, run the following commands

```shell
sudo apt-get update
sudo apt-get install nginx
```

6) Once this installation completes, check the installation by opening a web browser and checking the IP Address provided. You should be greeted with the following image...

![Alt-text](http://i.imgur.com/jGJghyJ.png)

7) Install MariaDB with the command...

```shell
apt-get install mariadb-server
```

Continue with the installation until you reach this screen...

![](http://www.morphatic.com/wp-content/uploads/2016/05/Screen-Shot-2016-05-20-at-4.20.05-PM.png)

Enter wordpress for the password and then wordpress again to confirm.

8) Once its finished installing, log into Maria with the following command...

```shell
mysql -u root -p
```

Notice that MariaDB is a direct MySQL replacement. Thus you use mysql commands to interact with it. You will be prompted to give the password to the MariaDB. Reply with the "wordpress" password.

9) You should see a prompt now that looks like...

```shell
MariaDB [(none)]>
```

Enter the following command

```shell
CREATE DATABASE wordpress;
```

10) Now that you have created the wordpress database, we are going to create a Database user and assign them privledges to the account we just created. You do this while still in the MariaDB interface with the following commands.

```shell
CREATE USER wordpressuser@localhost IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost;
FLUSH PRIVILEGES;
exit
```
11) Exit should push you back to the ssh active terminal window. We are now going to download wordpress tar to drop the wordpress project into the root's home directory. We do this with...

12) Create a new user and give that user superuser access.

```shell
 adduser wordpress
 usermod -aG sudo wordpress
```

13) We are going to be doing a lot quickly here. We are going to be creating a new directory and then using rsync to link directories together that nginx uses. We want to do this so we don't have to edit or work directly inside nginx's file structure to make sure we maintain it's intregedy. The following commands will create the new folders, sync the files and bridge everything over.

```shell
cd /var/www/html/
sudo chown -R wordpress:www-data /var/www/html/*
mkdir wp-content/uploads
sudo chown -R :www-data /var/www/html/wp-content/uploads
```

14) Install php7 with FPM

```shell
 sudo apt-get install php-fpm
```

15) We need to edit the php.ini file to remove a rather pesky security risk. Open your editor (vim in our case).

```shell
sudo vim /etc/php/7.0/fpm/php.ini
```
Find 


16) Now restart php7!

```shell
sudo service php7.0-fpm restart
```

17) We are going to be placing a new default server configuration.

```shell
sudo vim /etc/nginx/sites-enabled/default
```

Paste this into the new file...

```shell
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	root /var/www/html;

	# Add index.php to the list if you are using PHP
	index index.php index.html index.htm index.nginx-debian.html;

	server_name 104.131.167.10;

	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}

	# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
	#
	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
	
	#	# With php7.0-cgi alone:
	#	fastcgi_pass 127.0.0.1:9000;
	#	# With php7.0-fpm:
		fastcgi_pass unix:/run/php/php7.0-fpm.sock;
	}

	# deny access to .htaccess files, if Apache's document root
	# concurs with nginx's one
	#
	location ~ /\.ht {
		deny all;
	}
}
```
18) Restart the nginx!

```shell
sudo service nginx restart
```

19) Check out the installation page for more information on how to use Git-hooks to pull down your repo into this new server!