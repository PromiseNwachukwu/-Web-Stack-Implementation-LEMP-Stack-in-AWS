# WEB STACK IMPLEMENTATION (LEMP STACK)
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

## LEMP stack is now fully configured.
