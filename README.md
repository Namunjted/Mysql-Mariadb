## **Các lệnh sql cơ bản** 

#### **1. Đăng nhập MySQL bạn dùng lệnh:**
```
$ sudo mysql -u root -p
```

Mẫu đăng nhập có dạng : ```mysql -u {user_name} -p -h {ip_address} {db_name}```

Trong đó :
 - *user_name* : tên người dùng cơ sở dữ liệu . vd : root
 - *-h* và *ip_address* nếu bạn muốn truy cập vào cơ sở dữ liệu từ một máy tính khác.
 - *db_name* tên của database bạn muốn truy cập. (có thể để trống)

Khi đăng nhập thành công màn hình sẽ hiện ra cửa sổ thao tác như sau:

```
MariaDB [(none)]>
```

#### **2.Thư mục chứa database**

Trên CentOS hoặc Ubuntu , toàn bộ file raw database được lưu trong thư mục ```/var/lib/mysql```

#### **3.Quản lý tài khoản và phân quyền**
```
Hiển thị toàn bộ users:
mysql> SELECT User FROM mysql.user;


Tạo một user:
mysql> CREATE USER 'user'@'localhost' IDENTIFIED BY 'password_here';

Xóa tất cả user mà không phải root:
mysql> DELETE FROM mysql.user WHERE NOT (host="localhost" AND user="root");

Đổi tên tài khoản root (giúp bảo mật):
mysql> UPDATE mysql.user SET user="mydbadmin" WHERE user="root";

Gán full quyền cho một user mới:
mysql> GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' IDENTIFIED BY 'mypass' WITH GRANT OPTION;

Phân quyền chi tiết cho một user mới:
mysql> GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES ON mydatabase.* TO 'username'@'localhost' IDENTIFIED BY 'mypass';

Gán full quyền cho một user mới trên một database nhất định:
mysql> GRANT ALL PRIVILEGES ON mydatabase.* TO 'username'@'localhost' IDENTIFIED BY 'mypass' WITH GRANT OPTION;

Thay đổi mật khẩu user:
mysql> UPDATE mysql.user SET password=PASSWORD("newpass") WHERE User='username';

Xóa user:
mysql> DELETE FROM mysql.user WHERE user="username";

Cuối cùng reload user
mysql> FLUSH PRIVILEGES;
mysql> exit;
```
#### **4. Các thao tác database**
```
Hiển thị toàn bộ databases:
mysql> SHOW DATABASES;
Tạo database:
mysql> CREATE DATABASE mydatabase;

Sử dụng một database:
mysql> USE mydatabase;

Xóa một database:
mysql> DROP DATABASE mydatabase;
```
Tối ưu database:

All Databases:
```
$ sudo mysqlcheck -o --all-databases -u root -p
```
Single Database:
```
$ sudo mysqlcheck -o db_schema_name -u root -p
```
#### **5. Các thao tác table**
```
Tất cả các thao tác bên dưới bạn phải lựa chọn trước database bằng cách dùng lệnh:
mysql> USE mydatabase;

Hiển thị toàn bộ table:
mysql> SHOW TABLES;

Hiển thị dữ liệu của table:
mysql> SELECT * FROM tablename;

Đổi tên table :
mysql> RENAME TABLE first TO second;
hoặc
mysql> ALTER TABLE mytable rename as mynewtable;

Xóa table:
mysql> DROP TABLE mytable;
```

#### **6. Các thao tác cột và hàng**
```
Tất cả các thao tác bên dưới bạn phải lựa chọn trước database bằng cách dùng lệnh: mysql> USE mydatabase;

Hiển thị các column trong table:
mysql> DESC mytable;
hoặc
mysql> SHOW COLUMNS FROM mytable;

Đổi tên column:
mysql> UPDATE mytable SET mycolumn="newinfo" WHERE mycolumn="oldinfo";

Select dữ liệu:
mysql> SELECT * FROM mytable WHERE mycolumn='mydata' ORDER BY mycolumn2;

Insert dữ liệu vào table:
mysql> INSERT INTO mytable VALUES('column1data','column2data','column3data','column4data','column5data','column6data','column7data','column8data','column9data');

Xóa dữ liệu trong table:
mysql> DELETE FROM mytable WHERE mycolumn="mydata";

Cập nhật dữ liệu trong table:
mysql> UPDATE mytable SET column1="mydata" WHERE column2="mydata";
```

#### **7. Các thao tác sao lưu và phục hồi**
```
Sao lưu toàn bộ database bằng lệnh (chú ý không có khoảng trắng giữa -p và mật khẩu):
mysqldump -u root -pmypass --all-databases > alldatabases.sql

Sao lưu một database bất kỳ:
mysqldump -u username -pmypass databasename > database.sql

Khôi phục toàn bộ database bằng lệnh:
mysql -u username -pmypass < alldatabases.sql (no space in between -p and mypass)

Khôi phục một database bất kỳ:
mysql -u username -pmypass databasename < database.sql

Chỉ sao lưu cấu trúc database:
mysqldump --no-data --databases databasename > structurebackup.sql

Chỉ sao lưu cấu trúc nhiều database:
mysqldump --no-data --databases databasename1 databasename2 databasename3 > structurebackup.sql

Sao lưu một số table nhất định:
mysqldump --add-drop-table -u username -pmypass databasename table_1 table_2 > databasebackup.sql
```
### **Tài liệu nâng cao**

* [How to show list users in a mysql-mariadb database](https://www.cyberciti.biz/faq/how-to-show-list-users-in-a-mysql-mariadb-database/)

* [Cấp quyền cho user](https://mariadb.com/kb/en/library/grant/)
