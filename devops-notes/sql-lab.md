SQL lab

**Lab 1: Setting Up MySQL**

1. **Objective:** Install and configure MySQL on your server.
   - Download and install MySQL based on ubuntu.
   - Set up the initial configuration, including the root password.
   - Connect to MySQL using the command-line client.
     ```bash
     sudo apt install mysql-server -y
     sudo systemctl start --now mysql
     sudo mysql
     mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'toor';
     mysql> exit
     mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH auth_socket;
     mysql> exit
     sudo mysql_secure_installation
     sudo mysql
     ```
     ```sql
     CREATE USER 'localuser'@'localhost' IDENTIFIED WITH mysql_native_password BY 'toor';
     CREATE USER 'remote'@'%' IDENTIFIED BY 'toor';
     GRANT CREATE, ALTER, DROP, INSERT, UPDATE, INDEX, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'localuser'@'localhost' WITH GRANT OPTION;
     FLUSH PRIVILEGES;
     ```

**Lab 2: Creating a Database**

1. **Objective:** Learn how to create a new MySQL database.

   - Use the `CREATE DATABASE` statement to create a new database.
   - Verify the creation using the MySQL command-line client.
   - List existing databases with `SHOW DATABASES;`.

     ```sql
     CREATE DATABASE devktops;
     SHOW DATABASES;

     GRANT ALL PRIVILEGES ON devktops.* TO 'localuser'@'localhost' WITH GRANT OPTION;
     FLUSH PRIVILEGES;
     ```

**Lab 3: Creating Tables**

1. **Objective:** Create tables to structure your data.

   - Choose a database to work with using `USE`.
   - Create a table with appropriate columns, data types, and constraints using `CREATE TABLE`.
   - Use `DESCRIBE` or `SHOW COLUMNS FROM` to inspect the table structure.

     ```sql
     USE devktops;

     CREATE TABLE courses (
         id int NOT NULL,
         course_name varchar(250) NOT NULL,
         instructor_name varchar(250) NOT NULL,
         topic varchar(50) NOT NULL,
         PRIMARY KEY (id)
     );

     DESCRIBE courses;

     SHOW COLUMNS FROM courses;
     ```

**Lab 4: CRUD Operations**

1. **Objective:** Perform basic CRUD operations on your tables.

   - Insert data into the table using `INSERT INTO`.
   - Retrieve data using `SELECT`.
   - Update existing records using `UPDATE`.
   - Delete records using `DELETE FROM`.

     ```sql
     INSERT INTO
         courses (id, course_name, instructor_name, topic)
     VALUES
         (1, 'Git Basic', 'thixpin', 'DevOps'),
         (2, 'Linux System Administration', 'Mg KT', 'DevOps');

     SELECT * FROM courses;

     SELECT id, course_name  FROM courses;

     SELECT count(*) FROM courses;

     SELECT topic, count(*) FROM courses GROUP BY topic;

     SELECT id, course_name  FROM courses WHERE instructor_name = 'thixpin';

     UPDATE courses SET course_name = 'Docker' WHERE id = 2;

     SELECT * FROM courses;

     INSERT INTO
         courses (id, course_name, instructor_name, topic)
     VALUES
         (3, 'Linux System Administration', 'Mg KT', 'DevOps'),
         (4, 'Lambda', 'Mg KT', 'AWS');

     SELECT * FROM courses;

     SELECT * FROM courses ORDER BY id ASC;

     SELECT * FROM courses ORDER BY id DESC;

     SELECT * FROM courses ORDER BY id DESC LIMIT 2;
     SELECT * FROM courses ORDER BY id DESC LIMIT 1;
     SELECT * FROM courses ORDER BY id DESC LIMIT 2,1;

     DELETE FROM courses WHERE id = 2;

     SELECT * FROM courses;

     ```

**Lab 5: Backup and Restore**

1. **Objective:** Learn how to back up and restore databases.

   - Use `mysqldump` to create a backup of a database.
   - Restore the database from a backup using `mysql`.

     ```bash
     mysql -h localhost -P 3306 -u localuser -ptoor;
     mysql -h localhost -P 3306 -u localuser -p;
     mysqldump -h localhost -P 3306 -u localuser -p devktops > bk.sql
     mysqldump -h localhost -P 3306 -u localuser -p --no-tablespaces devktops > bk.sql
     ls -la
     sudo mysql -h localhost -P 3306
     > CREATE DATABASE restore_db;
     > exit
     mysql -h localhost -P 3306 -u localuser -p restore_db < bk.sql
     sudo mysql
     mysql> GRANT ALL PRIVILEGES ON restore_db.* TO 'localuser'@'localhost' WITH GRANT OPTION;
     mysql> FLUSH PRIVILEGES;
     mysql>exit
     mysql -h localhost -P 3306 -u localuser -p restore_db < bk.sql
     sudo mysql
     ```

     ```sql
     SHOW DATABASES;
     USE restore_db;
     SHOW TABLES;
     SELECT * FROM courses;

     REVOKE ALL PRIVILEGES ON restore_db.* FROM 'localuser'@'localhost';
     FLUSH PRIVILEGES;
     exit
     ```

     ```bash
     mysql -h localhost -P 3306 -u localuser -p restore_db < bk.sql
     ```
