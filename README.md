Run MySql in remote docker-container for JoGoVerein 4.06 with docker compose


- use mysql-connector-odbc-8.0.19-win32.msi on Win7
- runs on port 33061 instead of default 3306 !
- use root to copy database or create database from JoGoVerein
- switch to myuser for usage


docker-compose.yml:

version: '3.3'
services:
  db:
    image: mysql/mysql-server:8.0.19
    restart: always
    environment:
      MYSQL_DATABASE: 'my_database'
      MYSQL_USER: 'myuser'
      MYSQL_PASSWORD: 'mypassword'
      MYSQL_ROOT_PASSWORD: 'my_root_password'
      MYSQL_ROOT_HOST: '%'
    ports:
      - '33061:3306'
    expose:
      - '33061'
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ./mysql-data:/var/lib/mysql
      - ./mysql-conf:/etc/my.cnf.d
      - ./mysql-log:/var/log/mysql
      - ./initdb.sql:/docker-entrypoint-initdb.d/1.sql

--------------------
initdb.sql:

GRANT CREATE ON *.* TO 'root'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
------------------

mysql-conf/1.sql:

#general_log = 6
#log_slow_queries= /var/log/mysql/mysql-slow.log
#long_query_time=10
bind_addresss=0.0.0.0






