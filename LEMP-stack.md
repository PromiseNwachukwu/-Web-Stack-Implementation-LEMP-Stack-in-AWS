![Screenshot from 2023-09-28 14-16-29](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/0af29e6b-2021-44b6-a8d4-36d1c2b62f77)# WEB STACK IMPLEMENTATION (LEMP STACK)
## Step 0 - Preparing prerequisites
### Connecting to Ubuntu Server.
$ ssh -i LEMP_stack.pem ubuntu@44.201.246.18
![Screenshot from 2023-09-28 08-26-41](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/020c2260-6819-49c9-b1ca-be33ba147edc)

## Step 1 – Installing the Nginx Web Server
### To update all applications and install nginx.
$ sudo apt update
![Screenshot from 2023-09-28 09-06-48](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/0d709827-d705-4f3d-a33a-4bf23782349a)
$ sudo apt install nginx
![Screenshot from 2023-09-28 09-07-28](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/7cb8842d-1cef-4e82-83eb-d54f6ef91384)

### To verify if nginx was successfully installed.
$ sudo systemctl status nginx
![Screenshot from 2023-09-28 09-18-10](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/086fbbab-f6a8-4536-bad0-f6456ec150cf)

### Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web brousers use to access web pages in the Internet.
### To open TCP port 80 .
![Screenshot from 2023-09-28 09-25-46](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/28fca649-63f6-4ec3-9d76-d22520843805)

### To access our server locally in our Ubuntu shell, run:
$ curl http://localhost:80
### or
$ curl http://127.0.0.1:80
![Screenshot from 2023-09-28 09-31-38](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/c8fabeb4-1ecd-43e0-8e9a-7813303c604f)
![Screenshot from 2023-09-28 09-34-14](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/e823e4e9-0293-45ac-b71c-22b04174719f)

### To test if the server can respond to request via the internet
![Screenshot from 2023-09-28 09-40-17](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/fb991d15-6c38-48b0-8239-08ab334003e5)
### This reconfirms the curl output and confirms that the server is up and running.

## Step 2 — Installing MySQL
### MySQL is a popular relational Database Management System (DBMS) used within PHP environments for storing and managing data.
### To install MySQL
$ sudo apt install mysql-server
![Screenshot from 2023-09-28 09-51-03](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/d9caf925-c82b-4111-b530-e6521abbcd74)

### To connect to mysql server as an administrative database user root.
$ sudo mysql
![Screenshot from 2023-09-28 09-54-05](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/da4b094b-ed12-45ea-b38b-f73ba61dfe94)

### To run the security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Before running the script you will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord.1.
$ ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
![Screenshot from 2023-09-28 10-00-51](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/7a433040-437b-4b82-8a47-90450e31e6e0)

### Securing MySQL server  Validating password plugin.
![Screenshot from 2023-09-28 10-09-25](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/fcf8aca6-6ee6-4b68-a1e0-3bb130ae5773)
![Screenshot from 2023-09-28 10-09-25](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/f8636b6d-16e1-40ae-aa22-a5750e8db584)

### To test is we can log in to the MySQL console and exit afterwards.
$ sudo mysql -p
![Screenshot from 2023-09-28 10-30-49](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/11df8f39-19f7-4ef7-ad9b-155398a25c17)

### MySQL server is now installed and secured.

# Step 3 – Installing PHP
### PHP is installed to process code and generate dynamic content for the web server.
### Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install php-fpm(PHP fastCGI process manager), and tell Nginx to pass PHP requests to this software for processing. 
### Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
### To install these 2 packages at once:
$ sudo apt install php-fpm php-mysql
![Screenshot from 2023-09-28 10-45-52](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/c191d92a-92cb-4883-9917-bcc51beb8a87)
### The required PHP components are now installed.

## Step 4 — Configuring Nginx to Use PHP Processor.
### To create the root web directory for your_domain;
$ sudo mkdir /var/www/projectLEMP
![Screenshot from 2023-09-28 11-05-45](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/563ee45f-d1bf-4e28-8769-78fdcd193042)

### To assign ownership of the directory with the $USER environment variable, which will reference your current system user:
$ sudo chown -R $USER:$USER /var/www/projectLEMP

### To open a new configuration file in Nginx’s sites-available directory using nano.
$ sudo nano /etc/nginx/sites-available/projectLEMP
![Screenshot from 2023-09-28 11-12-30](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/57f40113-58fa-42e6-ac8f-690191a61885)

### To activate the configuration by linking to the config file from Nginx’s sites-enabled directory. This will tell Nginx to use the configuration next time it is reloaded. 
$ sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
![Screenshot from 2023-09-28 11-26-23](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/fefd0242-8fbc-44a2-b51b-048772f34c8c)

### To test the configuration for syntax errors:
$ sudo nginx -t
![Screenshot from 2023-09-28 11-26-23](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/fefd0242-8fbc-44a2-b51b-048772f34c8c)

### To disable default Nginx host that is currently configured to listen on port 80:
$ sudo unlink /etc/nginx/sites-enabled/default
![Screenshot from 2023-09-28 11-35-40](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/77288ff2-83f7-4a80-8436-70b8fbc66be5)

### To apply the changes, reload Nginx.
$ sudo systemctl reload nginx
![Screenshot from 2023-09-28 11-35-53](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/aad063c0-a7b5-4d4e-a499-faf83bd7a814)

### We now have an active website but the web root /var/www/projectLEMP is still empty.
### To  test and confirm that the new server block works as expected, create an index.html

### To connect using IP address.
$ http://44.201.246.18/
![Screenshot from 2023-09-28 11-53-28](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/a5160f29-4292-44ba-946c-dfd6a9c3abb0)

### To connect using DNS.
$ http://ec2-44-201-246-18.compute-1.amazonaws.com
![Screenshot from 2023-09-28 11-52-29](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/813a44c4-9c09-420c-98d0-5a66ad2f39da)

### LEMP stack is now fully configured and operational.

## Step 5 – Testing PHP with Nginx

### Testing the LAMP stack to validate that Nginx can correctly hand .php files off to your PHP processor.
$ Create a test PHP file in your document root.

$ Open a new file called info.php within your document root in your text editor:

$ nano /var/www/projectLEMP/info.php
![Screenshot from 2023-09-28 12-30-55](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/f64fd0f0-6115-4e2c-bb11-365acb815a1f)

### To access this page on the web browser by visiting the domain name or public IP address set up in the Nginx configuration file, followed by /info.php.
$ http://44.201.246.18/info.php
![Screenshot from 2023-09-28 12-34-34](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/9f312d94-c806-4890-9030-4187e12332e3)

### After checking the relevant information about the PHP server through that page, it’s best to remove the file created as it contains sensitive information about the PHP environment and Ubuntu server. This file cab always be regenerated if the need arises.
$ sudo rm /var/www/your_domain/info.php
![Screenshot from 2023-09-28 12-55-32](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/781d84fd-5e65-4291-99a2-14d36840d95b)

# Step 6 — Retrieving data from MySQL database with PHP
### First, to create a database named emeka_database and a user named emeka_user
$ mysql> CREATE DATABASE `emeka_database`;
![Screenshot from 2023-09-28 13-26-20](https://github.com/PromiseNwachukwu/my-portfolio1/assets/109115304/53c9e51d-d9d1-4293-a42f-736001fccc03)

### To create a user named emeka_user
$ mysql>  CREATE USER 'emeka_user'@'%' IDENTIFIED WITH mysql_native_password BY 'Password@1';
![Screenshot from 2023-09-28 13-32-49](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/a649cceb-b7ed-41aa-abf9-29f2cb5d61d1)

### To give emeka_user permission over the emeka_database database:
### This will give the emeka_user user full privileges over the emeka_database database, while preventing this user from creating or modifying other databases on your server.
$ mysql> GRANT ALL ON emeka_database.* TO 'emeka_user'@'%';
![Screenshot from 2023-09-28 13-39-43](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/78afffca-572b-4386-8df2-f3ac1ca45de7)

### To exit the MySQL shell with:
$ mysql> exit
![Screenshot from 2023-09-28 13-45-03](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/27616900-6fdf-4e64-befe-af928a648647)

### To test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:
$ mysql -u emeka_user -p
![Screenshot from 2023-09-28 13-46-58](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/ec9cb398-a8b4-4aa8-b555-f55473991f3a)

### To confirm that emeka_user has access to the emeka_database database:
$ mysql> SHOW DATABASES;
![Screenshot from 2023-09-28 13-50-55](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/c1007b00-f5ed-4129-855c-bfa38429acf2)

### To create a test table named todo_list. From the MySQL console, run the following statement:
$ CREATE TABLE emeka_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));
![Screenshot from 2023-09-28 13-54-15](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/8bd77d89-59c6-4fc9-9034-4342eb73946c)

### To insert a few rows of content in the test table. Repeat the command a few times, using different VALUES:
$ mysql> INSERT INTO emeka_database.todo_list (content) VALUES ("My first important item");
![Screenshot from 2023-09-28 14-02-05](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/4e62ec51-7d5c-42d6-8e63-9650bf510e5d)

### To confirm that the data was successfully saved tothe table:
$ mysql>  SELECT * FROM emeka_database.todo_list;
![Screenshot from 2023-09-28 14-07-38](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/8d09de81-5755-4946-8c18-f2ca5de0fcc5)

### To exit after confirmaton.
$ mysql> exit
![Screenshot from 2023-09-28 14-09-30](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/72af8567-3106-4eb2-9099-09d996b91a27)

### To create a PHP script that will connect to MySQL and query for your content. 
#### Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that:
$ nano /var/www/projectLEMP/todo_list.php
![Screenshot from 2023-09-28 14-16-29](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/3e30dbef-3611-4084-9660-aa461dc2845f)

### To access this page on the web browser by visiting the public IP or domain name address configured for your website, followed by /todo_list.php:
$ http://44.201.246.18/todo_list.php
![Screenshot from 2023-09-28 14-27-20](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/69ac507e-83e1-4f98-bbaa-e2ed72cf884a)

$ http://ec2-44-201-246-18.compute-1.amazonaws.com/todo_list.php
![Screenshot from 2023-09-28 14-29-17](https://github.com/PromiseNwachukwu/-Web-Stack-Implementation-LEMP-Stack-in-AWS/assets/109115304/07ed4e89-7966-424b-8e59-a1c23e5fda7b)

### The PHP environment is ready to connect and interact with your MySQL server.

### We have now succeeded in building a flexible foundation for serving PHP websites and applications, using Nginx as web server and MySQL as database management system.
