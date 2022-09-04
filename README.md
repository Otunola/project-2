# project-2  LEMP STACK IMPLEMENTATION
1. After the ubunter server creation on AWS,start off by updating your server’s package index with the following command : sudo apt update
2. then intall NgiNx  web server with the command : sudo apt install nginx
3. confirm the status of the installation with the command : sudo systemctl status nginx 
as shown below
<img width="562" alt="Screen Shot 2022-09-03 at 4 48 53 PM" src="https://user-images.githubusercontent.com/112595648/188312297-9238003b-4f2d-47d0-8e56-ceced8028ba7.png">

4. open the TCP port 80 of the EC2 instance on AWS as shown below so as to allow traffic flow on port 80, 

<img width="1109" alt="Screen Shot 2022-09-03 at 5 06 32 PM" src="https://user-images.githubusercontent.com/112595648/188312380-b413881f-d939-4239-9400-f856d6cfbea4.png">

5. checked if can access the port 80 from the ubuntu with the command : curl http://127.0.0.1:80
<img width="565" alt="Screen Shot 2022-09-03 at 5 19 06 PM" src="https://user-images.githubusercontent.com/112595648/188313080-f70d8f6a-7f44-48be-863c-f99365b86d05.png">

6. also try to access the server ip address on the URL

<img width="1095" alt="Screen Shot 2022-09-03 at 5 20 45 PM" src="https://user-images.githubusercontent.com/112595648/188313308-1af44cc6-1a7d-4b31-9d57-cce941377466.png">
7. Since NginX works well now we install a server to handle the database for the web server. we use mysql and will install with the command
 : sudo apt install mysql-server
8. after installation is finished, log in to the MySQL console by typing: sudo mysql

9.  Run a security script that comes pre-installed with MySQL. This  will remove some insecure default settings and put a password unto the database:
: ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
10. the password of the database is now set to PassWord.1

11.the database can now be accessed with the command : sudo mysql -p
 and the password will be required to access it because of the -p
 12. Your MySQL server is now installed and secured. Next, we will install PHP, the final component in the LEMP stack.

13. Since NginX is properly installed, Now you can install PHP to process code and generate dynamic content for the web server.
   the packages needed will be installed with this command : sudo apt install php-fpm php-mysql
14. the version of the php installation can be checked with the command : php -v
You now have your PHP components installed. Next, you will configure Nginx to use them.

 To encapsulate configuration details and host more than one domain on a single server. in this project we create another directory to add to the default one instead of modifying the default.
 
 15. the new directory is named projectLEMP and created witht the command : sudo mkdir /var/www/projectLEMP
 16. Assign ownership of the directory with the current user : sudo chown -R $USER:$USER /var/www/projectLEMP

We then use nano to create a new site for Nginx’s sites-available with the command : sudo nano /etc/nginx/sites-available/projectLEMP

17. We paste and save the following component
18. #/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}

19. Activate your configuration by linking to the config file from Nginx’s sites-enabled directory with the command 
    :sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/

20. then we test out NginX config so far with the command : sudo nginx -t
<img width="563" alt="Screen Shot 2022-09-04 at 1 10 33 AM" src="https://user-images.githubusercontent.com/112595648/188316794-117cfc0b-9dd5-4b90-bcc0-7c3261e47c3e.png">

21. Then disable default Nginx host that is currently configured to listen on port 80 with the command
    : sudo unlink /etc/nginx/sites-enabled/default
22 Reload Nginx for changes to take effect with the command : sudo systemctl reload nginx
23. Create an index.html file in that location so that we can test that your new server block works as expected with the comand
   : sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
   
24.Now go to your browser and try to open your website URL using IP address

<img width="1074" alt="Screen Shot 2022-09-04 at 1 15 46 AM" src="https://user-images.githubusercontent.com/112595648/188317638-4cf795ce-6e4b-4a07-b1f6-0c33899ef1d8.png">

25. Your LEMP stack is now fully configured, now we’ll create a PHP script to test that Nginx is in fact able to handle .php files within your newly configured website.
  can do this by creating a test PHP file in your document root. Open a new file called info.php within your document root in your text editor with the command
      : sudo nano /var/www/projectLEMP/info.php
26. save the php script in the created file

<?php
phpinfo();


27.enter the url adding /info.php to it and below seen


 <img width="1019" alt="Screen Shot 2022-09-04 at 1 29 46 AM" src="https://user-images.githubusercontent.com/112595648/188318459-2a0535d9-a317-48fb-80a4-58e9b1ed6dd8.png">

28. the .php contains alot of details and should be removed for some security reasons using command
 : sudo rm /var/www/your_domain/info.php

29. Now lEMP IS setup and done and a task here is to create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.

30. First, connect to the MySQL console using the root account using : sudo mysql -p
then enter the password when prompted.

<img width="568" alt="Screen Shot 2022-09-04 at 1 41 44 AM" src="https://user-images.githubusercontent.com/112595648/188319163-1ae6a111-5770-48fb-b36c-1ca8cfc793ce.png">

31. Create a new database called example-database with the command from mysql console: mysql> CREATE DATABASE `example_database`;

32. Create a new user and its password with the command: mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';

33. grants the users permissions over the database with the command : mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';

34. then exit from the mysql root... then login with the created example user and password with the command below
   : mysql -u example_user -p

<img width="560" alt="Screen Shot 2022-09-04 at 1 53 16 AM" src="https://user-images.githubusercontent.com/112595648/188319622-78f18616-c202-4ca2-95e5-b6701b44a67e.png">

35. mysql> SHOW DATABASES; gives the output below

<img width="558" alt="Screen Shot 2022-09-04 at 1 55 00 AM" src="https://user-images.githubusercontent.com/112595648/188319646-c854e202-014a-4223-ae4b-c1f3e85a88e1.png">

36. we’ll create a test table named todo_list. From the MySQL console, run the following statement :

CREATE TABLE example_database.todo_list (
item_id INT AUTO_INCREMENT,
content VARCHAR(255),
PRIMARY KEY(item_id)
);

![image](https://user-images.githubusercontent.com/112595648/188319791-15a18c6d-98b8-4e02-bfea-65ff11429e2c.png)

37. Insert a few rows of content in the test table in the daatabase with the command: 
INSERT INTO example_database.todo_list (content) VALUES
("My first important item"),
("My second important item"),
("My third important item"),
("and this one more thing");
![image](https://user-images.githubusercontent.com/112595648/188319895-191b5f29-0508-4952-b412-f3c0457fa745.png)

38. To confirm that the data was successfully saved to your table, run: mysql>  SELECT * FROM example_database.todo_list;
to get the output 
<img width="567" alt="Screen Shot 2022-09-04 at 2 12 18 AM" src="https://user-images.githubusercontent.com/112595648/188319966-0fb7a28b-33c0-4db5-a98e-8fce9b1a5299.png">

39. we then exit mysql
40.create a PHP script that will connect to MySQL and query for your content with the command : nano /var/www/projectLEMP/todo_list.php
41. copy and save the script below to the todo_list.php :
 <?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}

42. enter your server url /todo_list.php
<img width="747" alt="Screen Shot 2022-09-04 at 2 14 17 AM" src="https://user-images.githubusercontent.com/112595648/188320422-229ef5c8-8612-4cf9-9d63-3b7ed4226830.png">

