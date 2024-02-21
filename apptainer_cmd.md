# Use Apptainer on SuperPod/M3
Author: Junhao Shen
email: junhaos@smu.edu

- OS:MacOS/Linux  
- reference: https://apptainer.org/docs/user/latest/quick_start.html  
https://ponyland.science.ru.nl/ponyland/development_tips/mysql_with_apptainer/


## Mysql use case
### 1. Connect to SuperPod/M3
...

### 2. Create a file that contains an initial MySQL command that sets the root password:
`echo "SET PASSWORD FOR 'root'@'localhost' = '<root_password>';" > .mysqlrootpw`

Repalce root_password with your password

### 3. Create a new container instance by pulling the MySQL-Docker image:
`apptainer pull --name mysql.simg docker://mysql`

run `pwd` to get the current directory, and replace `${PWD}` with the output of `pwd` or PWD= the output of `pwd`

### 4. Start the MySQL container instance:
`apptainer instance start --bind ${PWD} --bind ${PWD}/mysql/var/lib/mysql/:/var/lib/mysql --bind ${PWD}/mysql/run/mysqld:/run/mysqld ./mysql.simg mysql`

`--bind` makes sure that some directories in the current directory are also available in the container.

### 5. Initialize the MySQL database:
`apptainer exec instance://mysql mysqld --initialize --init-file=${PWD}/.mysqlrootpw`

### 6. Run the daemon:
`apptainer exec instance://mysql mysqld --init-file=${PWD}/.mysqlrootpw`

check if the container is running on port 3306: `lsof -i:3306`
### 7. Connect to the MySQL container instance:
`apptainer shell instance://mysql mysql -S mysql/run/mysqld/mysqld.sock -u root -p`

Enter the password you set in step 2.


### Once you have complete above steps you only need to start your instance and run the daemon to start the mysql server by following step 4, 6, 7.
