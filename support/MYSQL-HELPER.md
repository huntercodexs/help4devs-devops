
# MYSQL HELPER

###### Database connection:

<pre>
mysql -h $hostname -u root -p
</pre>

###### Data import:

<pre>
mysql -h $hostname -u $user -p < $script.sql
</pre>

###### Data Dump:

<pre>
mysqldump -u $user -p $database > $filename.sql
</pre>

###### General Commands:

<pre>
CREATE DATABASE $DBNAME CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
USE $DBNAME;
SHOW TABLES;
CREATE TABLE $TABLENAME (FIELD1 varchar(2) DEFAULT NULL, FIELD2 int(20) DEFAULT NULL,FIELD3 int(20) DEFAULT NULL) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
SELECT * FROM $TABLE;
INSERT INTO $TABLE(@columns) values ('{@values}');
</pre>

###### User Create and Permissions:

<pre>
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
GRANT ALL PRIVILEGES ON *.* TO 'username'@'10.0.4.128';
GRANT ALL PRIVILEGES ON dbtest.* TO 'username'@'localhost';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'%';
GRANT USAGE ON *.* TO 'newuser'@'localhost';
GRANT USAGE ON *.* TO 'username'@'localhost';
GRANT USAGE ON *.* TO 'username'@'10.0.4.128';
GRANT USAGE ON *.* TO 'username'@'%';
GRANT USAGE ON *.* TO 'newuser'@'%';
FLUSH PRIVILEGES;
GRANT type_of_permission ON database_name.table_name TO 'username'@'localhost';
SHOW GRANTS FOR 'username'@'localhost';
DROP USER 'username'@'localhost';
REVOKE ALL PRIVILEGES ON BdAlunos.* FROM 'root'@'%';

[examples]

GRANT ALL PRIVILEGES ON dbtest.* TO 'username'@'%';
GRANT ALL PRIVILEGES ON dbtest.* TO 'username'@'localhost';
GRANT ALL PRIVILEGES ON dbtest.* TO 'username'@'10.0.4.128';

GRANT USAGE ON *.* TO 'username'@'%';
GRANT USAGE ON *.* TO 'username'@'localhost';
GRANT USAGE ON *.* TO 'username'@'10.0.4.128';

REVOKE ALL PRIVILEGES ON dbtest.* FROM 'username'@'localhost';
REVOKE ALL PRIVILEGES ON dbtest.* FROM 'username'@'10.0.4.128';
REVOKE ALL PRIVILEGES ON dbtest.* FROM 'username'@'%';
</pre>

