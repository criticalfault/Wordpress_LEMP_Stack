#Wordpress on LEMP Installation Guide

1) Login into Digital Ocean Account in order to create a new Droplet.

2) Create a new Droplet with *Ubuntu 14.04.5 x64*

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

```shell
cd ~
wget http://wordpress.org/latest.tar.gz
```

12) Now we need to use the "tar" command to unzip this package into its own directory. We do this with...

```shell
tar xzvf latest.tar.gz
```

13) Now we update apt-get because we installed new software and grab a few extra components that wordpress will use. Specifically things that will make it easier for us to grab plugins and components using SSH

```shell
sudo apt-get update
sudo apt-get install php5-gd libssh2-php
```
14) We have finished downloading and grabbing packages for our server so now we just need to do some configuration house cleaning. First we need to resolve the wordpress configuration file. Wordpress comes with a sample that we will be copying and then editing.

```shell
cd ~/wordpress
cp wp-config-sample.php wp-config.php
vim wp-config.php
```

15) Now that your in the guts of your wp-config file, you need to look for the MySQL settings. Edit the final lines to match this.

```shell
...
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'wordpressuser');

/** MySQL database password */
define('DB_PASSWORD', 'password');
. . .
```

16) Create a new user and give that user superuser access.

```shell
 adduser wordpress
 usermod -aG sudo wordpress
```

17) We are going to be doing a lot quickly here. We are going to be creating a new directory and then using rsync to link directories together that nginx uses. We want to do this so we don't have to edit or work directly inside nginx's file structure to make sure we maintain it's intregedy. The following commands will create the new folders, sync the files and bridge everything over.

```shell
sudo mkdir -p /var/www/html
sudo rsync -avP ~/wordpress/ /var/www/html/
cd /var/www/html/
sudo chown -R wordpress:www-data /var/www/html/*
mkdir wp-content/uploads
sudo chown -R :www-data /var/www/html/wp-content/uploads
```

18) Install php5 with FPM

```shell
 apt-get install php5-fpm php5-mysql
```

19) We are going to be placing a new wordpress server configuration.

```shell
sudo vim /etc/nginx/sites-available/wordpress
```

Paste this into the new file...

```shell
server {
	listen 80 default_server;
	listen [::]:80 default_server ipv6only=on;

	root /var/www/html;
	index index.php index.html index.htm;

	# Make site accessible from http://localhost/
	server_name localhost;

	location / {

		try_files $uri $uri/ /index.php?q=$uri&$args;
	}
	
	error_page 404 /404.html;

	# redirect server error pages to the static page /50x.html
	#
	error_page 500 502 503 504 /50x.html;
	location = /50x.html {
		root /usr/share/nginx/html;
	}

	# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
	#
	location ~ \.php$ {
		fastcgi_split_path_info ^(.+\.php)(/.+)$;

	#	# With php5-fpm:
		fastcgi_pass unix:/var/run/php5-fpm.sock;
		fastcgi_index index.php;
		include fastcgi_params;
	}
}
```

20) Link the created file to put it into the nginx's document structure. 

```shell
sudo ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/
```

21) Restart the server

```shell
sudo service nginx restart
sudo service php5-fpm restart
```

22) Log onto Wordpress and finish the configuration!
