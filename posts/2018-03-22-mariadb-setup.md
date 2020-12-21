title: "Mariadb Setup 101"
date: 2018-03-22 12:10:00 +0800
Category: Setup
Tags: Mariadb Setup, Database 
Has_Math: True

設定營運網站需求的 database 有很多細節要考慮,
但是對於測試需要的環境, 相對簡化很多步驟, 即便如此,
還是有很多新手在設定時遭受挫折, 包括筆者自己, 本篇提供一個 101 步驟,
一步步設定好測試用的 database.


### Environment ###

Debian 9 (Stretch)

Mariadb

### Reference sites ###

主要參考這兩篇的文章:

(1) [https://linode.com/docs/databases/mariadb/mariadb-setup-debian/](https://linode.com/docs/databases/mariadb/mariadb-setup-debian/)

(2) [https://websiteforstudents.com/configure-remote-access-mysql-mariadb-databases/](https://websiteforstudents.com/configure-remote-access-mysql-mariadb-databases/)

### Steps ###

完全按照(1)文件的步驟, root 密碼用 `123456` 直到進 database 畫面：

	Welcome to the MariaDB monitor.  Commands end with ; or \g.
	Your MariaDB connection id is 30
	Server version: 5.5.37-MariaDB-1~wheezy-log mariadb.org binary distribution
	
	Copyright (c) 2000, 2014, Oracle, Monty Program Ab and others.
	
	Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
	
	MariaDB [(none)]>

然後打 > quit; 離開, 這時確定我們有安裝了 mariadb. 記得 root 密碼 `123456`

接著編輯 `sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf`

	[mysqld]
	bind-address                               = 0.0.0.0

`0.0.0.0` 表示讓 remote user 可以登入使用.

重啟服務：
	
	sudo systemctl restart mariadb.service
	
檢查是否開放 remote 了:

	sudo netstat -anp | grep 3306
	
如果正確, 應該看到:

	tcp       0      0 0.0.0.0:3306          0.0.0.0:*        LISTEN         3213/mysqld

安裝 mariadb 需要的資訊:

	> sudo systemctl stop mariadb.service
	> sudo mysql_install_db
	> sudo systemctl start mariadb.service
	
接下來開放使用者 testuser 來使用測試的資料庫 testdb, 記得 使用者/密碼 要加單引號 `'xxx'`
密碼一樣是 `123456`:

	mysql -u root -p
	
	123456
	
	CREATE DATABASE testdb;
	CREATE USER 'testuser' IDENTIFIED BY '123456';
	GRANT ALL PRIVILEGES ON testdb.* TO testuser;
	FLUSH PRIVILEGES;
	
	quit;


如果使用 root:

	CREATE DATABASE testdb;
	GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456';
	FLUSH PRIVILEGES;
	
	quit

這樣, 遠端的 testuser 或 root 就可以連上來, 在 testdb 裡面加 table, 開始測試(亂搞)....

#



