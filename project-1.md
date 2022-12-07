## LAMP STACK IMPLEMENTATION(PROJECT 1)

We are setting up a LAMP STACK using EC2 as our virtual server.
To do this we need to do the following:

* create an account on [AWS](www.aws.amazon.com). 
* we create an instance (virtual machine) by selecting __“ubuntu server 20.04 LTS”__ from Amazon Machine Image(AMI)(free tier). 
* we select “t2.micro(free tier eligible)” 
* then go to the security group and select “existing security group” review and launch.

 [how to create an aws free tier account](https://www.youtube.com/watch?v=xxKuB9kJoYM&list=PLtPuNR8I4TvkwU7Zu0l0G_uwtSUXLckvh&index=7)

This launches us into the instance as shown in the screenshot:

[aws instance](./images/instance-launch.PNG)

We open our terminal and go to the location of the previously downloaded PEM file:

[locate PEM file](./images/cd-downloads-to-locate-the-pem.PNG)

We connect to the instance from our ubuntu terminal using the command:

> git add .
> ```ssh -i dybran-ec2.pem ubuntu@44.210.117.5```

[connecting to the instance](./images/anot.PNG)

This automatically connects to the instance

[instance connected](./images/b.PNG)

__APACHE WEB SERVER SETUP__

To install the Apache web server, we first update the software packages in the software package manager by running the command:

> ```sudo apt update```

[updating package manager](./images/c.PNG)

Next we install the web server(Apache HTTP server) by running the command:

> ```sudo apt install apache2```

[apache server installation](./images/install-apache.PNG)

To check if the webserver  is running, we use the command:

> ```sudo systemctl status apache2```

[apache status](./images/apache-status.PNG)

We then go to our instance and set the inbound rules and save

[Inbound rules set up](./images/inbound-rules.PNG)

We copy the URL and paste on a web browser to check if it is working:

[checking apache URL](./images/checking-apache-url.PNG)

__MYSQL DATABASE SERVER SET UP__

We install the Mysql server by running the command:

> ```sudo apt install mysql-server```

[mysql server installation](./images/install-mysql.PNG)

We log into the mysql by running the command:

> ```sudo mysql```

[mysql log in](./images/sudo-mysql.PNG)

to set password as "PassWord.1" for root user, we run the command:

> ```ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';```

[setting root user password](./images/mysql-alter.PNG)

Then we “exit” the mysql and then run the command to start the interactive script:

> ```sudo mysql_secure_installation```

[starting the script](./images/abc.PNG)

[coninuation](./images/def.PNG)

Logged into mysql using my new password by running the command:

> ```sudo mysql -p```

[logging in using my new password](./images/ac.PNG)

Then “exit”.

__SETTING UP PHP__

To install PHP, we run the command:

> ```sudo apt install php libapache2-mod-php php-mysql```

We check the version by running the command:

> ```php -v```

[installing php and version check](./images/qwe.PNG)

__CREATING A VIRTUAL HOST FOR APACHE__

To create a virtual host for my website files using apache, we create a directory “projectlamp”
Then change ownership :

> ```sudo mkdir /var/www/projectlamp```
 
> ```sudo chown -R $USER:$USER /var/www/projectlamp```

[creating directory and ownership change](./images/ownership.PNG)

Created a new configuration file by running the command:

>```sudo vi /etc/apache2/sites-available/projectlamp.conf```

We place the following in the file we created

```
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
[projectlamp configuration file](./images/qa.PNG)

We save using __":wq" + "Enter"__ 

We enable the new virtual host

> ```sudo a2ensite projectlamp```

We disable the default website that comes with apache.

> ```sudo a2dissite 000-default```

To make sure our configuration files does not contain syntax error:

> ```sudo apache2ctl configtest```

We reload the apache webserver

> ```sudo systemctl reload apache2```

We create a file in /var/www/projectlamp named  “index.html”

> ```touch /var/www/projectlamp/index.html```

We then redirecting these data into the file:

> ```echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html```

[virtual host set up countinuation](./images/aq.PNG)

we can check on the browser using the url

> ```http://<Public-IP-Address>:80```

[display on browser](./images/er.PNG)

To enable PHP on the website, we open this file and edit it:

> ```sudo vim /etc/apache2/mods-enabled/dir.conf```

[dir.conf file](./images/vi.PNG)

we use __":wq" + "Enter"__ to save.

Create and open another file in “projectlamp” named “index.php”

> ```vim /var/www/projectlamp/index.php```

Add this to the file

```
<?php
phpinfo();
```

[index.php file](./images/php.PNG)

“Save” the file, then go to the browser to refresh

[php display](./images/php-site.PNG)

We remove the “index.php” file created in “projectlamp:

> ```sudo rm /var/www/projectlamp/index.php```

### we have successfully deployed a __LAMP STACK WEBSITE__ on __AWS CLOUD__
































